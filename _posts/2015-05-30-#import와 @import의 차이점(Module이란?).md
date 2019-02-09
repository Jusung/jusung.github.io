---
layout: post
title: "#import와 @import의 차이점(Module이란?)"
tags: 
  - Objective-C
comments: true
published: true
---

## 개요 

Objective-C 언어를 사용하면서 가장 자주 사용하는 전처리기 명령어로 `#import`가 있습니다. 이와 매우 비슷한 `@import`라는 명령어도 있는데 이 둘의 차이점은 무엇일까요? 이 둘간의 차이점을 이야기 전에 `#import` 명령어의 전신인 `#include`에 대해서 알아보도록 하겠습니다. 

## #include 
`#include` 명령어는 기본적으로 전처리기에 `#include`의 대상이 되는 파일을 `#include`가 있는 위치에 넣으라고 알려줍니다. 예를 한번 들어보죠. `IncludeMe.h` 파일이 있고 내용은 아래와 같습니다. 

```
// IncludeMe.h
#define kMyConstantNumber 42
#define kMyConstantBoolean true
```
 자 이제 이 헤더파일에 있는 상수를 이용해서 아래와 같이 C코드를 작성했습니다. 


```
// MyProgram.c
#include <stdio.h>
#include "IncludeMe.h"
printf("The constant is %d and the boolean is %d", kMyConstantNumber, kMyConstantBoolean);
```
`MyProgram.c` 코드에서는 어떻게 `printf()` 함수에서 출력 인자값으로 사용되는 `kMyConstantNumber` 와 `kMyConstantBoolean` 값에 대해서 알 수 있는 걸까요? 

`MyProgram.c`코드는 컴파일을 시작하기 전에 전처리기가 `IncludeMe.h` 파일의 내용을 `MyProgram.c` 코드에 삽입합니다. 그래서 실제로 컴파일러가 프로그램을 컴파일할 때 사용하는 소스는 아래와 같습니다. 


```
// MyProgram.c
#define kMyConstantNumber 42
#define kMyConstantBoolean true
printf("The constant is %d and the boolean is %d", kMyConstantNumber, kMyConstantBoolean);
```

매우 간단한 소스이긴 하지만 전처리기가 어떻게 동작하는지 확인하는데는 손색이 없습니다. 사실, 위 소스가 전체 소스는 아닙니다. 왜냐하면 `stdio.h` 의 내용은 포함되지 않았기 때문이죠. 실제로 컴파일 될때는 `stdio.h`의 내용도 복사 됩니다. 

참고로 `#include` 뒤에 “” 혹은 <> 로 감싸여진 파일을 적을 수 있는데, “”로 감싸여 진 것은 `IncludeMe.h` 파일을 `MyProgram.c` 파일 위치와 동일한 디렉토리에서 찾으라고 전처리기에 알려주는 것이고 `<stdio.h>` 와 같이 <>로 감싸진 파일은 시스템 헤더 디렉토리에서 찾으라는 것입니다. 

## 반복 포함(Recursive Includes) 

이렇듯 `#include` 지시자는 참 편리한 명렁어입니다. 이것을 이용해서 코드를 모듈화 할 수 있고, 재사용성을 향상 시킬 수 있습니다. 하지만 만약 이런 파일이 있으면 무슨 일이 발생할까요? 

```
// FirstFile.h
#include "SecondFile.h"
/ Some code /

// SecondFile.h
#include "FirstFile.h" 
/ Some other code /
```

전처리기는 처음에 `FirstFile.h` 를 처리하면서 `SecondFile.h` 파일의 내용을 포함시키려고 할 것입니다. 하지만 `SecondFile.h` 의 내용을 가지고 오려고 그 내용을 읽을 때 `SecondFile.h` 파일 내에 있는 `FirstFile.h` 의 내용을 포함하려고 할 것입니다. 그래서 다시 `FirstFile.h` 로 가서 그 내용을 가지고 오려고 할때, `SecondFile.h` 의 내용을 확인하러 가고.... 그림이 그려지시죠? 이것을 **반복 포함(Recursive Include)**이라고 합니다. 

## `#include` vs `#import` 
Objective-C에서는 이 반복 포함(Recursive Include)문제를 `#import` 지시어를 이용해서 해결했습니다. `#import` 지시어를 사용하면 헤더 파일이 정의되어 있는지 한번 확인해 보고 만약 정의되어 있다면 다음 번에는 그냥 건너뛰게 됩니다. 

C에서도 반복 포함 문제를 해결하기 위해 아래와 같은 전처리기 명령어를 지원하는데요. `#import`와 본질적으로는 동일합니다. 

```
#ifndef MyFile_h
#define MyFile_h
// Some code 
#endif
```

## @import 

`@import(모듈)`는 2012년 11월 애플의 WWDC에서 처음 소개됐는데요 전처리기의 다음 2가지 문제를 해결하기 위해 만들어낸 지시어 입니다. 

1.  부서지기 쉬움(Fragility)
2.  확장성(Scalability) 부서지기 쉬움(Fragility)과 관련해서 다음과 같은 예를 들 수 있습니다. 

```
// MyFile.h
#define strong @"this won't work"
#import <UIKit/UIKit.h>
@interface MyFile : NSObject
@property (nonatomic, strong) NSArray anArray;
@end
```
`strong` 이라는 예약어를 `#define`으로 정의 한다면 전처리기가 동작하고 나서 무슨일이 일어날까요? `MyFile.h`의 헤더파일 내용은 다음과 같이 될 것 입니다. 

```
// MyFile.h 
#define strong @"this won't work"
// UIKit imports
@interface MyFile : NSObject
@property (nonatomic, @"this won't work") NSArray anArray; 
@end
```
보시다 시피 `strong`이라는 예약어가 재정의 돼서 예약어가 의도치 않은 문자로 변경이 되었습니다. 

확장성(Scalability)과 관련해서는 `#import`가 본래 가지고 있는 문제가 있습니다. `#import`로 어떤 헤더 파일을 첨부한다고 가정했을 때, 그 헤더 파일 안에 `#import`가 있을 수 있습니다. 그러면 또 그 헤더 파일에 `#import`가 있는지를 살펴보고 만약 있다면 다시 한번 `#import`를 해야 합니다. 이와 같은 일을 `#import`가 헤더파일에 없을 때까지 계속 `#import` 를 해야합니다. 다시말해 컴파일시 `#import`로 선언된 여러 헤더파일의 코드를 헤더파일에 복사 붙여넣기를 해야한다는 것이고 그만큼 컴파일 시간이 증가합니다. 만약 `UIKit`이나 `Foundation`같이 거의 필수적으로 쓰이는 헤더파일이라면 비록 소스 코드가 매우 작더라도 헤더파일 때문에 컴파일 시간이 오래 걸리게 됩니다. 배보다 배꼽이 더 큰 상황인것이죠. 

Objective-C가 생긴지 꽤 오래됐는데 이 문제와 관련해 해결책을 찾아보려는 노력이 없었을까요? 네, 당연히 있었습니다. 그래서 생겨난게 <a href="http://en.wikipedia.org/wiki/Precompiled_header" target="_blank">Precompiled header</a>(.pch)라는 것입니다. 말그대로 전처리 기간에 헤더파일을 미리 컴파일해서 캐시에 저장해 두어서 실제 컴파일시 걸리는 시간을 단축할 수 있습니다. 뿐만아니라 이 컴파일되어 캐시된 헤더파일을 프로젝트의 모든 코드에서 사용할 수 있습니다. 

하지만 이 pch파일을 이용해서 Precompiled header를 만드는 방법도 여러 문제가 있습니다. 

1.  대다수의 개발자들은 .pch파일을 관리하지 않습니다. 처음에 조금 관리하다가 곧 관리를 중단합니다.

2.  프로젝트에 새로 투입된 개발자가 .pch에 등록되어 있는 모든 헤더파일이 어떤 관계인지 이해하기 어렵습니다.

3.  모든 곳에 포함되는 파일을 사용하고 싶지 않은 경우도 있을 것입니다. 

모듈(@import)을 사용하면 헤더파일 내용을 복사 붙여넣기하는 방법에서 벗어나, 마치 프레임워크를 캡슐화 해서 사용할 수 있습니다. 프레임워크의 컴파일은 한번만 되고 이후 라이브러리가 사용될때마다 효율적으로 import 할 수 있습니다. 게다가 모듈을 이용하면 앞에서 예를든 예약어 오버라이딩을 막을 수 있습니다. 왜냐하면 먼저 import 된 라이브러리에 해당 키워드가 등록되어 있기 때문입니다. 애플은 여기서 한발 더 나아가 전에는 프레임워크를 사용하기 위해서 link 설정에 프레임워크를 추가해줘야 했던 것을 더 이상 하지 않아도 되도록 만들었습니다. 다시말하면 모듈을 사용하면 프레임워크가 자동으로 링크됩니다. 

이 모듈을 한마디로 표현하자면 **해더-> 캐싱-> 자동링킹** 이라고 할 수 있을 것 같습니다. 

잠깐! 그러면 모듈을 사용하기 위해서 기존에 `#import`로 선언했던 코드를 수동으로 모두 `@import`로 바꿔야 하는건가요? 아닙니다. 간단히 옵션만 변경해주면 됩니다. 프로젝트 -> Build Settings -> modules로 검색하면 아래와 같이 `Enable Modules` (C and Objective-C)항목과 `Link Frameworks Automatically` 항목이 나옵니다.
![](https://lh3.googleusercontent.com/3VfMQPQjcawC5TfcV98Lh8An5g4tuzsBuNowbcoUiV_aVq0rnB2y8mJWuknoprVEHLFv-BqWBiFqhIgs1Fc8pOZtAHqZKlhxEl8FhH-0LtF6szf5SFzzaYNKWbM69o1-nUjE-LhQFH_lVMW07j6J4b2h677tT9R0mvWJth7TCsKY_Z51VHwle2Hw5dIH3iKf4_yu1RURv5mdeYI5o2ekVrFkA5e2-aJH5PROCFL_QEs6E3_Ay8mYGwPTMXP-QtlZ1JWLKYxNZsXV34fPK6eiHM_pwQk2U0P2un-e9CnovKR22NAhr0VcTpjG5mgYCQQbDwjNldmhDPQsFH4rIeW9PXpeRfa_0Q6sI-q25jqWZJneBkrV7G7ZoCKVmnFRXSZbMZpJLUFmmO5yWp-4-pzo9ORAaDSmC0F6csjJ4t-LSiNo2L06x9KCabHxgKlxm8ULbMF_sy-KASKx76W4HCq89gLvBqXCa8UK9MvIl9zuJwgyKhuFgjKB3dqnhy9-C3ToYp5Q4NhiJsdVKdygFsGa6_pQxMkEItSDtETSP2w5amMYWZ31RkPxxypA8ojYF2s47VNicQNK9SDhMXc0o6q21mMIWIHDnxNoAdtnDhdY2BKuOc_VbsAis04hBwcleexP=w1024-h314-no){: .center-image}

`Enable Modules`를 `Yes`로 설정하면 자동으로 `#import`를 `@import`로 매핑해서 `#import` 지시어로 `@import` 동작을 하도록 합니다. 

마지막으로 모듈을 사용하면 작은 프로젝트나 파일은 모르겠지만 큰 프로젝트는 체감할 수 있을 정도로 컴파일 시간이 단축됩니다. 컴파일 시간을 줄이고 싶으시다면 모듈을 사용하시면 도움이 될 것입니다. 

이상으로 모듈에 관해서 알아보았습니다. 


[참 고] 

1. [Introduction to Objective-C Modules](https://stoneofarc.wordpress.com/2013/06/25/introduction-to-objective-c-modules/)
2. [What’s New in Objective-C and Foundation in iOS 7](https://www.raywenderlich.com/2647-what-s-new-in-objective-c-and-foundation-in-ios-7)