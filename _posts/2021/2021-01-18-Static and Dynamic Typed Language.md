---
layout: post
title: "[Basic] 정적타입 언어와 동적타입 언어"
category:
  - Basic
tags:
  - Static, Dynamic
comments: true
published: true
---

## 정의
정적타입(Static Typed) 언어와 동적타입 언어( Dynamic Typed)를 구분하는 기준은 코드의 상수, 변수, 함수 등에 대한 타입을 언제 확인하는지입니다.  이 타입을 **코드를 실행하기 전**에 확인하는 언어를 **정적타입 언어**라 하고 **코드를 실행하고 나서** 확인하는 언어를 **동적타입 언어**라 합니다.

그럼 Swift는 어떤 타입의 언어일까요? 

Swift는 정적타입 언어입니다. 컴파일 시점에 코드의 타입을 검사해 타입이 맞지 않는 경우 컴파일러가 에러를 발생시킴니다.

## 각 타입의 장단점
두 타입을 구분짓는 요소인 타입을 어느 시점에 확인하느냐에 따라 각 타입의 장단점이 나뉩니다.
아래 코드는 동적타입 언어 사용의 예입니다.

```python
var x = [1, 2, 3] // Array
x = True      // Bool
x = "Hello"   // String
x = 100       // Int

```

변수 `x`의 타입은 런타임에 확인하기 때문에 최초 `x`에 배열을 넣은 후 `Bool`, `String`, `Int`등으로 변환해 사용해도 아무 문제가 없습니다.

정적타입 언어는 이와 반대입니다.

```swift
// 1.컴파일 에러발생!
var x = [1, 2, 3] 
x = true ❌
x = "Hello" ❌
x = 100 ❌

// 2.에러발생하지 않음
var x = [1, 2, 3]
var y: Bool = true
var z: String = "Hello" 
var a = 100 
```

정적타입 언어는 타입이 한번 결정되면 변하지 않고, 컴파일시 타입을 확인하기 때문에 동적타입 언어처럼 사용할 수 없습니다.

### 정적타입 언어의 장점 
이처럼 정적타입 언어는 컴파일시 타입에 대해 미리 확인하기 때문에 타입 관련한 런타임 오류를 방지할 수 있습니다. 특히 사용자에게 배포되는 앱의 경우 타입 관련한 검증을 컴파일타임에 하지않고 런타임에 하게 되면 앱사용시 타입 불일치로 인한 크래시가 발생 위험이 높아집니다.

정적타입 언어의 또 다른 장점은 코드의 가독성 입니다. 타입이 명시적으로 지정돼 있기 때문에 타입을 머리로 계산해 읽어야 하는 동적타입 언어에 비해 코드의 가독성이 좋습니다. 그래서 다수의 협업이나 프로젝트의 장기계발 및 유지보수에 유리합니다.

### 동적타입 언어의 장점 
그럼 동적타입 언어는 단점만 있을까요? 당연히 그렇지 않습니다. 

정적타입 언어는 사용하기 위해 알아야 할 타입 관련한 BoilerPlate코드와 지켜야할 규칙이 많습니다. 그래서 코드가 동적타입 언어에 비해 상대적으로 길어질 수 있습니다. 반면 동적타입언어는 사용하기 위해 지켜야할 규칙이 적기 때문에 상대적으로 코드가 짧고 Learning-Curve가 낮습니다.

## 정리
이상으로 정적타입 언어와 동적타입 언어에 대해 알아 보았습니다.

👨🏻‍💻지식이 +1 늘었다.  다음 포스트에서 또 만나요 🚀😄

**[참 고]**

- [Swift Statically Typed](https://learn.co/lessons/swift-statically-typed)
- [Static Typing and Type Inference](https://www.aidanf.net/learn-swift/types_and_type_inference) 
- [Static Types vs Dynamic Types](https://instil.co/blog/static-vs-dynamic-types/) 
- [Is Swift a dynamic or static language? - Stack Overflow](https://stackoverflow.com/questions/29924477/is-swift-a-dynamic-or-static-language) 
- [How do we determine a language is dynamic or static? an example is Swift - Stack Overflow](https://stackoverflow.com/questions/55213572/how-do-we-determine-a-language-is-dynamic-or-static-an-example-is-swift/55227001#55227001) 
- [Why Use a Dynamic Language over a Statically Typed One?](https://erik-engheim.medium.com/why-use-a-dynamic-language-over-a-statically-typed-one-fb434994e2b6) 
- [Is Python Dynamically Typed Language?](https://www.tutorialspoint.com/is-python-dynamically-typed-language) 