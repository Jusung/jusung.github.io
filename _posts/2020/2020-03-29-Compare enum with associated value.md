---
layout: post
title: "[Swift] 관련 값이 있는 Enum Case 비교하기"
category: 
  - Swift
tags: 
  - Enum
comments: true
published: true
---

Swift에서 `Enum`은 상태를 관리하기에 유용한 타입입니다. Enum의 case에 관련 값(Associated Value)을 사용하면 상태와 관련된 처리를 보다 편리하게 할 수 있습니다. 

이번 포스트에서는 관련 값을 사용하는 Enum case를 비교하는 방법을 알아 보겠습니다. 

비교를 위해 우선 Enum case에서 관련 값을 사용하지 않는 경우부터 살펴보겠습니다.

### 1. case에 관련 값을 사용하지 않는경우
```swift
enum State {
    case ready
    case notReady
}

let stateA = State.ready

if stateA == .ready {
    print("Ready")         // ✅ Ready가 결과로 출력 
} else {
    print("Not Ready")
}
```
관련 값을 사용하지 않는 경우에는 `if stateA == .ready` 와 같이 case와 case를을 직접 비교할 수 있습니다.
너무나도 직관적인 문법이죠.

### 2. case에 관련 값을 사용하는경우

관련 값을 사용하는 경우에는 비교 방법이 조금 달라집니다.

```swift
enum State {
    case ready
    case loading(String)
    case error(String)
}

let stateA = State.ready
```
case에 관련 값을 추가했습니다. 이제 앞서 했던 문법으로 case로 비교를 해보죠.

```swift
if stateA == .ready {	 //❌ 컴파일 에러 발생! 
    print("Loading")        
} else {
    print("Not Loading")
}
```
당연히 될꺼라 기대되지만 Enum case에 관련 값이 있는 타입이기 때문에 컴파일 에러가 발생하며 비교할 수 없습니다.

관련 값이 있는 case가 비교 대상인 경우는 어떨까요?

```swift
if stateA == .loading(_) {     // ❌ 컴파일 에러 발생! 
    print("Loading")
} else {
    print("Not Loading")
}

if stateA == .loading("Loading") {     // ❌ 컴파일 에러 발생! 
    print("Loading")
} else {
    print("Not Loading")
}
```
관련 값이 있는 경우도 마찬가지 입니다. 

관련 값을 사용하는 경우에는 다음 문법을 사용해 비교할 수 있습니다.

```swift
if case .ready = stateA {
    print("Ready")     // ✅ Ready가 결과로 출력
} else {
    print("Not Ready")
}

if case .loading(_) = stateA {
    print("Loading")     
} else {
    print("Not Loading") // ✅ Not Loading이 결과로 출력
}
```

`if case .loading(_) = stateA` 과 같이 문법이 마치 "왼쪽의 case의 틀에 오른쪽 case를 할당이 가능하냐?" 는 의미로 이해가 됩니다.  

이상 관련 값을 사용하는 Enum case를 비교하는 법에 대해 알아 보았습니다. (👨🏻‍💻지식이 +1 늘었다.)

다음 포스트에서 또 만나요~ 🚀😄 

[참  고]

- [Automatically derive properties for enum cases](https://forums.swift.org/t/automatically-derive-properties-for-enum-cases/10843)
- [Comparing enum cases while ignoring associated values](https://forums.swift.org/t/comparing-enum-cases-while-ignoring-associated-values/15922/2)
- [How to compare enum with associated values by ignoring its associated value in Swift?](https://stackoverflow.com/questions/31548855/how-to-compare-enum-with-associated-values-by-ignoring-its-associated-value-in-s)
