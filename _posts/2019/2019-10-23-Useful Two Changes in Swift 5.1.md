---
layout: post
title: "Swift 5.1의 유용한 변화 2가지"
category:
  - Swift 5.1
comments: true
published: true
---

이번 포스트에서는 Swift 5.1의 유용한 변화 2가지에 대해 살펴보겠습니다.

## 1. 암시적 리턴 (Implicit returns)
1) `Swift 5.1`부터 함수나 클로저 안에  `return` 키워드를 생략 가능합니다.[^1] 단, 코드블럭 안에  `return`이 오직 한번만 사용될 때 생략 가능합니다. 보다 자세한 내용은 이 후 내용에서 확인하실 수 있습니다.

```swift
struct Student {
    func whatToDo() -> String {
        "Just Play!"	// 매소드에서 return 키워드가 없이 값 반환 가능
    }
}
```

2) `computed-property`에서 다른 클래스의 매소드나 프로퍼티를 사용하는 곳에도 적용될 수 있습니다.

```swift
struct Student {
    var wannaGoCompany: String {
        Apple.name()	// 다른 클래스의 매소드도 return 키워드 없이 반환 가능
    }
}

struct Apple {
    static func name() -> String {
        "Apple Inc."
    }
}
```

2) 삼항연산자에서도 `return`을 한번 사용하기 때문에 `return` 을 생략 가능합니다.

```swift
struct Student {
    var isSenior = true

    func whatShouldIDo() -> String {
        isSenior ? "Study Hard!" : "Just Play"
    }
}
```

3) `if-else`  구문 등  `return`을 두번이상 사용하는 곳에서는 `return`을 생략할 수 없습니다.

```swift
struct Student {
    var isSenior = true
    
    func whatToDo() -> String {
        if isSenior {
            return "Study Hard!"
        }
        else {
            return "Just Play"
        }
    }
}
```

## 2. 맴버 초기자에 디폴트값 지원
클래스나 구조체에 프로퍼티를 선언하고 초기자를 따로 지정하지 않으면, 아래 코드와 같이 자동으로 모든 프로퍼티를 포함하는 초기자를 사용할 수 있었습니다.

```swift
struct Company {
    var name: String
    var employees: Int
    var isFAANG: Bool
}

let company = Company(name: "Apple", employees: 345000, isFAANG: true)
```

하지만 다음과 같이 프로퍼티에 초기값이 있는 경우도 자동으로 생성된 초기자 사용시에는 모든 프로퍼티 값을 할당해야 했습니다.

```swift
struct Company {
    var name: String
    var employees: Int = 345000
    var isFAANG: Bool = true
}
```

```swift
let company = Company(name: "Apple", employees: 345000, isFAANG: true)
// Swift 5.1 미만에서 자동 초기자의 사용
```

Swift 5.1에서는 자동 초기자에 디폴트 값을 지원해서 다음과 같이 사용할 수 있습니다.[^2]

```swift
let company = Company(name: "Apple") // 디폴트 값이 있는 프로퍼티는 초기화시 값할당이 생략 가능합니다.
```

이상 `Swift 5.1`에서 변경된 유용한 점 2가지를 살펴보았습니다. 😎

[더 자세히 알아보기]

관련내용을 더 자세히 보고 싶으신 분은 다음 Swift Evolution 제안 링크를 참조하세요.

[^1]: [Implicit returns from single-expression functions](https://github.com/apple/swift-evolution/blob/master/proposals/0255-omit-return.md){:target="_blank"}

[^2]: [SE-0242 Synthesize default values for the memberwise initializer](https://github.com/apple/swift-evolution/blob/master/proposals/0242-default-values-memberwise.md){:target="_blank"}