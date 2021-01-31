---
layout: post
title: "[Swift] Escaping 클로저 (@escaping)"
category:
  - Swift
tags:
  - Closure
comments: true
published: true
---


## 정의
Escaping 클로저는 클로저가 함수의 인자로 전달됐을 때, 함수의 실행이 종료된 후 실행되는 클로저 입니다. Non-Escaping 클로저는 이와 반대로 함수의 실행이 종료되기 전에 실행되는 클로저 입니다.

## Non-Escaping Closure 
그럼 이 클로저(closure)는 어떤 클로저 일까요?

```swift
func runClosure(closure: () -> Void) {
 closure()
}
```

클로저가 실행되는 순서를 보면

1. 클로저가 `runClosure()` 함수의 `closure` 인자로 전달됨
2. 함수 안에서 `closure()` 가 실행됨
3. `runClosure()` 함수가 값을 반환하고 종료됨

이렇게 클로저가 함수가 종료되기 전에 실행되기 때문에 `closure`는 Non-Escaping 클로저 입니다.

## Escaping Closure
이 경우는 어떨까요?

```swift
class ViewModel {
    var completionhandler: (() -> Void)? = nil
    
    func fetchData(completion: @escaping () -> Void) {
        completionhandler = completion
    }
}
```

클로저가 실행되는 순서를 보면

1. 클로저가 `fetchData()` 함수의 `completion` 인자로 전달됨
2. 클로저 `completion`이 `completionhandler` 변수에 저장됨
3. `fetchData()` 함수가 값을 반환하고 종료됨
4. 클로저 `completion`은 아직 실행되지 않음  

`completion`은 함수의 실행이 종료되기 전에 실행되지 않기 때문에 escaping 클로저, 다시말해 함수 밖(escaping)에서 실행되는 클로저 입니다. 

escaping 클로저가 사용되는 흔한 예로는 비동기로 실행되는 HTTP Request CompletionHandler이 있습니다.

```swift
func makeRequest(_ completion: @escaping (Result<(Data, URLResponse), Error>) -> Void) {
  URLSession.shared.dataTask(with: URL(string: "http://jusung.github.io/")!) { data, response, error in
    if let error = error {
      completion(.failure(error))
    } else if let data = data, let response = response {
      completion(.success((data, response)))
    }
  }
}
```

`makeRequest()` 함수에서 사용되는 `completion` 클로저는 함수 실행 중에 즉시 실행되지 않고, URL 요청이 끝난 후 비동기로 실행 됩니다. 이 경우에도 `completion`의 타입에 `@escaping` 을 붙여서 escaping 클로저라는 것을 명시해줘야 합니다.

보통 클로저가 다른 변수에 저장되어 나중에 실행되거나 비동기로 실행될 때 escaping 클로저가 사용됩니다.

## Non-Escaping Closure 와 Escaping Closure
그럼 함수에 `@escaping` 가 붙은 클로저에는 반드시 escaping 클로저만 인자로 사용해야 할까요?

그렇지는 않습니다. `@escaping`이 붙어 있어도  다음과 같이 non-escaping  클로저를 인자로 넣을 수 있습니다.

```swift
func runClosure(closure: @escaping () -> Void) {
 closure()  // ✅ closure는 non-escaping 클로저이지만 @escaping 사용 가능
}
```

반대로 escaping 클로저를 `@escaping` 없이 사용할 수 없습니다.

```swift
class ViewModel {
    var completionhandler: (() -> Void)? = nil
    
    func fetchData(completion: () -> Void) { // ❗️@escaping 누락으로 컴파일 에러 발생!
        completionhandler = completion
    }
}
```

그렇다면 여기서 한가지 궁금한점이 생깁니다. 🤔

> 클로저 파라미터 타입에 `@escaping`을 명시하면 escaping 클로저와 non-escaping 클로저를 모두 사용할 수 있는데, 그러면 그냥 모든 클로저 파라미터 타입에 `@escaping` 를 붙여서 사용하면 되지 않나?

> 아니 한걸음 더 나가서 복잡하게 non-escaping, escaping 나누지 말고 전부 escaping으로 통일시키고 따로 `@escaping`을 명시 하지 않아도 Swift에서 모든 클로저의 파라미터 타입을 escaping으로 간주해서 처리하면 될 것 같은데. 뭐하러 escaping, non-escaping을 나누는거지?

이렇게 escaping, non-escaping 클로저를 나눠서 사용하는 이유는 컴파일러의 퍼포먼스와 최적화 때문입니다. 

non-escaping 클로저는 컴파일러가 클로저의 실행이 언제 종료되는지 알기 때문에, 때에 따라 클로저에서 사용하는 특정 객체에 대한 retain, release 등의 처리를 생략해 객체의 라이프싸이클(life-cycle)을 효율적으로 관리할 수 있습니다.

반면 esacping 클로저는 함수 밖에서 실행되기 때문에 클로저가 함수 밖에서도 적절히 실행되는 것을 보장하기 위해, 클로저에서 사용하는 객체에 대한 추가적인 참조싸이클(reference cycles) 관리 등을 해줘야 합니다. 이 부분이 컴파일러의 퍼포먼스와 최적화에 영향을 끼치기 때문에 Swift에서는 필요할 때만 escaping 클로저를 사용하도록 구분해 두었습니다.

## 정리
이번 포스트에서는 "Escaping 클로저 (@escaping)" 에 대해 살펴봤습니다.

👨🏻‍💻지식이 +3 늘었다. 다음 포스트에서 또 만나요 🚀😄

**[참 고]**

- [Closures](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)

- [escaping closures](https://developer.apple.com/forums/thread/71633)

- [What is @escaping in Swift?](https://www.donnywals.com/what-is-escaping-in-swift/)

- [Swift 3.0에 추가된 @noescape와 @escaping에 대해 살펴보자!](http://papasmf.blogspot.com/2016/12/swift-30-noescape-escaping.html)

- [[swift] escaping closure](https://jintaewoo.tistory.com/41)

- [What do mean @escaping and @nonescaping closures in Swift?](https://medium.com/swiftcommmunity/what-do-mean-escaping-and-nonescaping-closures-in-swift-d404d721f39d)

- [Escaping & Non-Escaping Closures, Memory Management, and You](https://buildingvts.com/escaping-non-escaping-closures-memory-management-and-you-cb936c60a9d1)

- [What is Escaping And Non-Escaping Closure in Swift?](https://www.c-sharpcorner.com/article/what-is-escaping-and-non-escaping-closure-in-swift/)

- [How To: Escaping Closures In Swift With @escaping](https://learnappmaking.com/escaping-closures-swift/)

- [Escaping and Non-Escaping Closures in Swift](https://medium.com/better-programming/escaping-and-non-escaping-closures-in-swift-fe2866309599)