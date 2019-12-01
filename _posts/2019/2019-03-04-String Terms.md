---
layout: post
title: "String 관련 용어 정리"
category: 
  - Swift
tag:
  - Basic
comments: true
published: true
---

용어의 정의를 제대로 알아야 관련 글이나 문서를 읽을 때 정확한 이해가 가능합니다. 
이번 포스트에서는 Swift에서 사용되는 `String`관련 용어에 대해 알아보겠습니다.

* **String literals**(문자 리터럴) : 소스에 있는 문자열을 나타냅니다.

```swift
let stringLiteral = "Hello, World!"	
```

* **String delimiters**(문자 경계) : 문자열에서 시작과 끝의 경계를 구분짓는 문자. Swift에서는 `큰따옴표(")`를 문자열의 경계로 사용합니다.

```swift
let myWord = "Hello, World!" // Hello, World!를 감싸고 있는 큰따옴표(")가 String delimiter 입니다.
```
* **Escape characters**(이스케이프 문자) : `String literal` 안에서 특별하게 해석되는 단일문자 혹은 문자열로 Swift에서는 `백슬래시(\)`(U+005C)를 사용합니다.

```swift
let income: Int = 500
let incomeInWon = "\(income)원" // \(income)
```

* **Escape character sequences**(이스케이프 문자 시퀀스) : 줄여서 Escape sequence라 부릅니다. `Escape character`가 앞에 붙은 문자열로 이 문자는 문자 그대로 표현되지 않고 컴파일러에 의해 다른 문자로 변환됩니다. 문자를 직접 표현하는게 불가능하거나 어려울 때 사용됩니다.
Swift는 다음과 같은 `Escape sequence`를 지원합니다.

```swift
* \0 : null 문자
* \\ : 백슬래시(\)
* \n : 라인피드
* \r : 케리지 리턴 (carriage return)
* \” : 큰따옴표
* \’ : 작은따옴표
* \u{*n*} : 임의의 유니코드 스칼라 값. (n은 1~8자리의 16진수)
```
Swift에서는 위의 `Escaping Sequence`를 사용한 [예]

```swift
* let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
* let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
* let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
* let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```
이상으로 Swift의 `String` 관련 용어에 대해 알아보았습니다.

[참고]

* [String and Characters](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html)
* [Swift Strings](https://www.codingexplorer.com/swift-strings/)
* [Escape sequences in C - Wikipedia](https://en.wikipedia.org/wiki/Escape_sequences_in_C)