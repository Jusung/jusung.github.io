---
layout: post
title: "[Swift] 클래스만 사용 가능한 프로토콜의 선언"
category: 
  - Swift
tags: 
  - Protocol, Build Error
comments: true
published: true
---


```
'weak' must not be applied to non-class-bound 'MyDelegate'; consider adding a protocol conformance that has a class bound
```

이런 에러가 발생했다구요? 이 에러는 `class`를 따르지 않는 프로토콜을  `weak` 변수로 선언해 사용할 때 발생하는 에러입니다.

예를들어 보겠습니다. 

프로토콜을 이용해 델리게이트를 선언합니다.

```swift
protocol MyDelegate {
    func runDelegateMethod()
}
```

클래스 안에서 위에서 선언한 델리게이트 프로토콜을 따르는 `delegate`를 `weak`로 선언 합니다. `weak`로 선언한 이유는 Retain Cycle을 피하기 위해서 입니다.

```swift
class ClassProtocolTest: MyDelegate {
    weak var delegate: MyDelegate?
    func runDelegateMethod() {}
}
```

이 코드를 빌드하면 앞서 언급한 에러가 발생합니다. 왜일까요?

프로토콜은 클래스와 구조체, 열거형에 사용 가능합니다. 앞에서 선언한 프로토콜은 클래스에서 사용되는지 아니면 구조체나 열거형에서 사용되는지 알 수 없기 때문에 `reference count`관리를 위해 사용되는 `unowned`이나 `weak` 키워드를 사용할 수 없는 것입니다.

이 에러를 해결하기 위해서는 프로토콜을 `class`를 따르도록 선언해서 클래스에서만 사용 가능하도록 명시합니다.

```swift
protocol MyDelegate: class {
    func runDelegateMethod()
}
```

이렇게 선언하면 에러 없이 빌드가 가능합니다.

만약 이 클래스에서만 사용가능한 프로토콜을 아래와 같이 구조체에서 사용하려고 하면 

```swift
struct ClassProtocolTest: MyDelegate {
    weak var delegate: MyDelegate?
    func runDelegateMethod() {}
}
```

다음과 같이 클래스 타입이 아닌 곳에서 클래스 타입 프로토콜을 따를 수 없다는 에러가 발생합니다. 

```
Non-class type 'ClassProtocolTest' cannot conform to class protocol 'MyDelegate'
```