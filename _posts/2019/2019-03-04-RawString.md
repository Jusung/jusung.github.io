---
layout: post
title: "[RxSwift] Raw String"
category:
  - Swift
tag:
  - Swift 5.0
comments: true
published: true
---

Swift에서 `String`을 사용할 때 누구나 겪었을 만한 불편함이 하나 있습니다.
그건 바로 문자열에서 `"`나 `\`를 표현하기 위해 `\`를 추가로 사용해야 하는 것이죠.

간단한 (예)를 살펴 보겠습니다.

표현하고 싶은 문자는 `I love "You"` 입니다. You를 `"`로 감싸기 위해 `"` 다음과 같이 앞에 `\`를 추가해야 합니다.

```swift
let love = "I love \"You\""
```

정규식을 사용하는 경우는 더 복잡해 집니다. 정규식 자체도 약간의 복잡함을 갖고 있는데, 여기에 `\`가 추가되면 보기에 복잡해 보입니다. 아래 정규식은 `\Location.latitude` 같은 키패스 찾는 정규식입니다.

정규식 자체는 `\\[A-Z]+[A-Za-z]+\.[a-z]+` 이렇게 표현되지만, 실제 코드에서 사용하려면 다음과 같이 표현 해야합니다.

```swift
let regex1 = "\\\\[A-Z]+[A-Za-z]+\\.[a-z]+"
```

보시다시피 `\`가 여러개 추가돼서 복잡해 보입니다. 이렇게 사용된 정규식은 정규식 검증이나, 다른 언어에 복사해서 사용하기 위해 바로 복사해 사용할 수 없고 `\`를 직접 발라서 사용해야합니다.

그렇다면 `"`나 `\`를 사용하기 위해 `\`를 추가하는 이런 추가적인 작업은 왜 필요한것 일까요? <br/>
그 이유는 `"`와 `\`가 각각 문자열의 시작과 끝을 나타내는 경계 문자(String delimiters)와 이스케이프 문자(Escape characters)로 사용되기 때문입니다. 

다시 말해 `"`는 문자열의 범위의 경계를 파악하기 위한 기호로 사용하고, `\`는 이스케이프 문자로 문자열(string literals) 안에서 특별한 일을 처리하기 위해 사용하기 때문입니다.

이런 불편함을 해결하고자 Swift 5.0[(SE-0200)](https://github.com/apple/swift-evolution/blob/master/proposals/0200-raw-string-escaping.md) 에서는 문자열의 경계를 나타내는 새로운 경계 문자 `#"..."#` 를 추가한 **Raw String** 을 지원합니다. 무슨 말인지 잘 모르시겠다구요? 예제를 보겠습니다.

위에 들었던 예제를 한번 살펴보죠.

```swift
let love1 = "I love \"You\""
let love2 = #"I love "You""#
```

비교가 되시나요? 문자열을 큰따옴표 대신 `#"문자내용"#` 로 감싸고 이 문자내용에 `"`나 `\`를 추가적인 `\` 없이 사용할 수 있습니다.

앞에서 살펴봤던 정규식을 보죠.

```swift
let regex1 = "\\\\[A-Z]+[A-Za-z]+\\.[a-z]+"
let regex2 = #"\\[A-Z]+[A-Za-z]+\.[a-z]+"#
```

Raw String 문법을 이용해 regex2 같이 추가적인 `\`없이 정규식을 표현할 수 있습니다.

그렇다면 기존의 문자열 안에 상수나 변수의 내용을 넣고 싶을때 사용했던 `\(property)`는 어떨까요?
다음과 같이 `\#(property)`로 표현할 수 있습니다.

```swift
let price = 1000
let guide = #"The price of the shoes is \#(price)."#
```

앞에서 나왔던 것들을 종합해서 사용한 예제는 다음과 같습니다.

```swift
let url = "https://apple.com"

let html1 = "<a href=\"\(url)\" title=\"Apple Developer\">"
var html2 = #"<a href="\#(url)" title="Apple Developer">"#
```

어떤신가요? **Raw String**을 사용하면 좀더 보기 편하지 않나요?
**Raw String**을 사용하면 이제 더 이상 추가적인 `"`나, `\`를 사용할 필요가 없습니다. Yay! 😆

잠깐! 그럼 만약 **Raw String** 안에 `"#`(문자열 끝 경계문자)을 써야하는 경우는 어떨까요?
그런 경우에는 문자열 경계 문자를 `#"..."#` 가 아닌 `##"..."##`를 사용하면 됩니다. 그렇게 해서 다음과 같이 표현할 수 있습니다.

```swift
let str = ##"My cat is "Good"#GoodDog"##
```

결과 : `My cat is Good#GoodDog` 


**[Swift String에서 경계 문자(Delimiter)의 사용]**<br/> 

| String Start Delimiter | Escape Delimiter | String End Delimiter  |
| ---------------------- | -----------------| --------------------- |
| `"`                    | `\`			    | `"`                   |
| `#"`	                  | `\#`             | `"#`                  |
| `##"`	               | `\##`            | `"##`                 |
| `######"`	            | `\######`        | `"######`             |

이상으로 Swift 5.0에서 추가된 **Raw String**에 대해 알아보았습니다.
**Raw String**을 사용해 사전 정의된 경계문자 `"`, `\` 등이 포함된 문자를 이제 보다 쉽고 편하게 사용할 수 있습니다. **Raw String** 덕분에 고통을 덜 수 있겠네요. <br/> 
다음 포스트에서 또 만나요~ 😊

### 남은 이야기 (Raw String 문법 디자인 뒷 이야기)

Swift의 **Raw String** 문법을 디자인할때 [Rust](https://ko.wikipedia.org/wiki/러스트_(프로그래밍_언어))에서 영감을 받았는데, `Rust`에서 사용하는 `r"..."` 나 이것을 변형한 `raw"..."`  `#raw"..."` `#raw("...")` 등을 고려해봤는데 안예쁘고 텍스트 콘텐츠 집중을 방해해서 결국 `#"..."#` 스타일로 결정했다고 하네요. `String`을 사용할때 `#raw"..."` 이런식으로 사용해야 한다고 생각하면 끔찍하네요. 좋은 의미로 역시 🍎스럽네요. 애플 굳! 👍
 
### 참고

* [SE-0200 - Enhancing String Literals Delimiters to Support Raw Text](https://github.com/apple/swift-evolution/blob/master/proposals/0200-raw-string-escaping.md)
* [Swift 5 Release Notes for Xcode 10.2 beta 3](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_2_beta_3_release_notes/swift_5_release_notes_for_xcode_10_2_beta_3)
* [What’s new in Swift 5.0](https://www.hackingwithswift.com/articles/126/whats-new-in-swift-5-0)
