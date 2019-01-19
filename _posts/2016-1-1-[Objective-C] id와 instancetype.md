---
layout: post
title: "[Objective-C] id와 instancetype"
tags: 
  - Objective-C
comments: true
published: true
---

과거 Objective-C에서는 아래와 같이 객체를 생성하고 초기화 하는 `alloc`과 `init` 매소드의 반환형으로(return type)으로 `id`형(type)을 사용했던 적이 있습니다.

```objc
@interface NSObject
+ (id)alloc;
- (id)init;
@end

@interface NSArray : NSObject
@end
```
 

`id`형은 모든 클래스로 전환이 가능하기 때문에, 생성되거나 초기화된 객체를 다형성(polymorphism)의 이점을 살린 상태에서 반환하는 것이 가능했습니다.

하지만 아래의 예와 같이 `id`를 사용하는 것의 결점이 존재 했습니다.

```objc
NSArray *array = [[NSDictionary alloc] init];
NSArray *array = [[[NSArray alloc] init] undefinedMethod];
```

예전에는 컴파일시 위의 두 줄의 코드에서 오류가 발생하지 않았습니다.

첫번 째 줄 코드는 `init`의 반환형이 `NSDictionary`가 되어야 함에도 불구하고 `id`형으로 반환이 되기 때문에 생성된 객체를 `NSArray`객체에 할당하는 잘못된 코드에 대해 컴파일러가 오류를 발견하지 못했습니다.

두번 째 줄 코드도 클래스에 선언되지 않은 매소드를 호출함에도 불구하고 컴파일러는 `id`형에서 호출하는 것으로 이해하기 때문에 여기에서도 오류를 발견하지 못했습니다.

 

그래서 나온 것이 `instancetype`입니다. `instancetype` 오직 반환 값에만 사용할 수 있는 타입입니다.

`instancetype`이 반환형으로 선언되면 항상 호출하는 클래스와 같은 형을 반환하게 됩니다. 그렇게 함으로써 다형성의 이점도 유지하면서 형의 안정성을 높일 수 있습니다. 이것이 `instancetype` 형이 나온 배경입니다.

 

현재는 `alloc`, `init` 혹은 `new`로 시작하면서 `id` 형을 반환 형으로 가지고 있는 매소드는 컴파일러가 자동적으로 `instancetype`으로 변환합니다. 이 경우가 아닌 경우에는 명시적으로 `instancetype`을 선언해 주어야 합니다.

이상으로 `instancetype` 형에 대해서 알아봤습니다.