---
layout: post
title: "Bool 값을 반전시키는 toggle()"
category: 
  - Swift
tag:
  - 4.2
comments: true
published: true
---

## 개요
Swift 4.2에서 `Bool`값을 반전시키는 `toggle()`이라는 함수가 `Bool`에 추가 되었습니다.[^1] 이 포스트에서는 새로 추가된 `toggle()`함수에 대해 알아 보겠습니다.

## toggle()
코딩을 하다보면 `Bool`값을 반전시켜야 하는 경우가 있습니다. 보통은 다음과 같이 구현합니다.

```swift
myVar.property1.property2.enabled = !myVar.property1.property2.enabled
```

특히, `Bool`까지의 경로가 긴 경우 위와 같이 같은 경로를 반복해서 적어줘야 하는 불편함이 있었습니다. 이런 상황에서 편리하게 사용할 수 있도록 `toggle()`이라는 함수가 `Bool`에 추가 됐습니다. 그래서 이제 같은 기능을 아래 코드로 구현할 수 있습니다.

```swift
myVar.property1.property2.enabled.toggle()
```

실제 코드를 작성해 실행해 보겠습니다.

**[소스코드]**

```swift
var usePushNotification = false
print("Noti State Before Toggle : \(usePushNotification)")
    
usePushNotification.toggle()
print("Noti State After Toggle : \(usePushNotification)")
```

**[실행결과]**

```
Noti State Before Toggle : false
Noti State After Toggle : true
```

## toggle()의 구현
Swift 표준 라이브러이에 추가된 `toggle()`함수는 다음과 같이 구현돼 있습니다.

```swift
extension Bool {
  mutating func toggle() {
    self = !self
  }
}
```

이상으로 Swift 4.2에서 추가된 `toggle()`함수에 대해 알아 보았습니다.

[^1]: [SE-0199 Adding toggle to Bool](https://github.com/apple/swift-evolution/blob/master/proposals/0199-bool-toggle.md){:target="_blank"}