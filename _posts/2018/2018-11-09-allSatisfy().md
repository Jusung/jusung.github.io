---
layout: post
title: "[Swift] allSatisfy() 매소드"
category: 
  - Swift
tag:
  - Swift 4.2
comments: true
published: true
---

코딩을 하다보면 콜렉션의 모든 원소가 특정 조건을 만족시키는지 확인하고 싶을 때가 있습니다. 
Swift 4.2에서는 이런 경우 사용할 수 있는 `allSatisfy()` 매소드를 지원합니다.[^1]

만약 문자로 구성된 콜렉션에서 모든 원소가 특정 문자를 포함하는지 확인하기 위해 `allSatisfy()` 매소드를 다음과 같이 사용할 수 있습니다.

```swift
let aWords = ["Apple", "Approve", "Analytics"]
let allMatch = aWords.allSatisfy { $0.hasPrefix("A") }
```

숫자 콜렉션에서도 다음과 같이 사용할 수 있습니다.

```swift
let numbers = [28, 32, 64, 90]
let passed = numbers.allSatisfy { $0 >= 28 }
```

이상으로 Swift 4.2에서 추가된 `allSatisfy()` 매소드에 대해 알아 보았습니다. 😎

[^1]: [SE-0207 Add an allSatisfy algorithm to Sequence](https://github.com/apple/swift-evolution/blob/master/proposals/0207-containsOnly.md){:target="_blank"}