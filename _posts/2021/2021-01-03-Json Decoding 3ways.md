---
layout: post
title: "[Swift] Codable을 사용해 json 데이터 디코딩 방법 3가지"
category:
  - swift
tags:
  - Codable
comments: true
published: true
---  

RESTful API를 사용하는 앱 개발시 가장 흔하게 하는 작업 중 하나는 서버에서 내려주는 json 데이터를 인코딩하거나 디코딩하는 일입니다. Swift 4에서 지원하기 시작한 [Codable](https://developer.apple.com/documentation/swift/codable) 프로토콜을 사용하면 json 데이터를 쉽게 인코딩/디코딩 할 수 있습니다.

이번 포스트에서는 `Codable`(Decodable)을 사용해 json 데이터를 디코딩하는 3가지 방법에 대해 알아 보겠습니다.

## 1. 기본적인 방법

[Decodable](https://developer.apple.com/documentation/swift/decodable)을 사용해 json 데이터를 디코딩 하는 가장 기본적인 방법은 다음과 같이 모델 정의시 json 데이터의 key와 이름이 같고 value와 타입이 같은 프로퍼티를 선언하는 것입니다.

[json data]

```json
{
    "name": "tommy's blog",
    "postsCount": 20,
    "imageUrl": "https://jusung.github.io",
    "views": 1000,
    "tagLine": "Happy Swift Coding"
}
```

[모델]

```swift
struct Blog: {
    let name: String
    let postsCount: Int
    let imageUrl: URL
    let views: Int
    let tagLine: String
}
```

선언한 모델을 보면 json의 key와 모델의 프로퍼티 이름이 일치하고 json value의 타입과 프로퍼티의 type이 일치하를 것을 확인할 수 있습니다.

이제 `Blog`모델이 `Decodable` 프로토콜을 준수(conform)하도록 지정하고 디코딩을 수행합니다.

[코드]

```swift
// json 데이터
let json = """
{
    "name": "tommy's blog",
    "postsCount": 20,
    "imageUrl": "https://jusung.github.io",
    "views": 1000,
    "tagLine": "Happy Swift Coding"
}
""".data(using: .utf8)!

// 모델이 Decodable을 준수하도록 지정
struct Blog: Decodable {
    let name: String
    let postsCount: Int
    let imageUrl: URL
    let views: Int
    let tagLine: String
}

// 디코딩
let blog: Blog = JSONDecoder().decode(Blog.self, from: json)
```

[실행결과]

```
Blog(name: "tommy\'s blog", postsCount: 20, imageUrl: https://jusung.github.io, views: 1000, tagLine: "Happy Swift Coding")
```

결과를 확인하면 json 데이터대로 `Blog`모델이 잘 생성 됐음을 확인할 수 있습니다.

## 2. 수동으로 key를 지정하는 방법

Swift에서는 코딩 네임 컨벤션을 Apple이 권장(**[Swift API Design Guide](https://swift.org/documentation/api-design-guidelines/#general-conventions)** )하는 camel-case를 사용합니다.  앞서 살펴본 예제에서는 json 데이터의 key가 camel-case로 돼 있어서 프로퍼티 이름을 camel-case 형태로 유지한채 디코딩을 할 수 있었습니다. 

하지만 만약 json 데이터의 key가 camel-case가 아닌 다른 형태로 돼 있다면 어떻게 할까요?

예를 들어, json 데이터가 snake-case로 돼 있다고 한번 해보죠.

```json
{
    "name": "tommy's blog",
    "posts_count": 20,
    "image_url": "https://jusung.github.io",
    "views": 1000,
    "tag_kine": "Happy Swift Coding"
}
```

이 경우에는 앞에서 살펴본 1번 방법으로 디코딩을 하기 위해서는 모델을 snake-case로 수정해야합니다.

```swift
struct Blog: Decodable {
    let name: String
    let posts_count: Int
    let image_url: URL
    let views: Int
    let tag_line: String
}
```

하지만 이렇게 되면 일관된 코딩 컨밴션을 깨드리게 됩니다. 이건 저희가 원하는 방법이 아닙니다.

`Codable` 프로토콜을 준수하는 타입은 다음과 같이 `String`과 `CodingKey`를 준수하는 CodingKey라는 `enum`을 선언할 수 있습니다. 이 방법을 사용하면 수동으로 모델의 프로퍼티와 json 데이터의 key/value를 매핑시켜 디코딩을 할 수 있습니다.

```swift
 enum CodingKeys: String, CodingKey {
    case name
    case postsCount = "posts_count"
    case imageUrl = "image_url"
    case views
    case tagLine = "tag_line"
}
```

`CodingKeys`를 선언하고 디코딩을 수행합니다.

[코드]

```swift
struct Blog: Decodable {
    let name: String
    let postsCount: Int
    let imageUrl: URL
    let views: Int
    let tagLine: String

    enum CodingKeys: String, CodingKey {
        case name
        case postsCount = "posts_count"
        case imageUrl = "image_url"
        case views
        case tagLine = "tag_line"
    }
}

let json = """
{
    "name": "tommy's blog",
    "posts_count": 20,
    "image_url": "https://jusung.github.io",
    "views": 1000,
    "tag_line": "Happy Swift Coding"
}
""".data(using: .utf8)!

let blog: Blog = JSONDecoder().decode(Blog.self, from: json)
```

[실행결과]

```
Blog(name: "tommy\'s blog", postsCount: 20, imageUrl: https://jusung.github.io, views: 1000, tagLine: "Happy Swift Coding")
```
snake-case로 돼 있는 json도 camel-case형태로 잘 디코딩 된 것을 확인할 수 있습니다.

## 3. Decoder에 Stratege를 사용하는 방법 (snake-case를 camel-case로 자동으로 변환하는 방법)

앞서 살펴본 “2. 수동으로 key를 지정하는 방법” 을 사용하면 어떠한 형태의 json 데이터가 들어와도 디코딩하는데는 지장이 없습니다.  만약 서버에서 내려준 snake-case 형태의 json 데이터의 key/value가 30개 인 경우는 어떨까요? 

2번 방법으로 디코딩을 하기 위해서는 개발자가 일일이 snake-case를 camel-case로 매핑해주기 위해 `CodingKey`를 구현해야합니다.  휴… 생각만해도 끔찍합니다.

서버에서 내려주는 데이터가 snake-case로 일관성이 있는데, 이 부분을 어떻게 자동화 할 수 없을까요?

네! 있습니다.

`JSONDecoder`에 [keyDecodingStrategy](https://developer.apple.com/documentation/foundation/jsondecoder/keydecodingstrategy)를 지정하면 자동으로 snake-case를 camel-case로(혹은 그 반대인 camel-case를 snake-case로) 매핑시켜 디코딩을 할 수 있습니다.  `keyDecodingStrategy`로`.convertFromSnakeCase`를 사용하면 각 데이터는 다음과 같이 변경됩니다.

```
원본: fee_fi_fo_fum
변경: feeFiFoFum 
원본: feeFiFoFum
변경: feeFiFoFum 
원본: base_uri
변경: baseUri
```

`JSONDecoder`에 `keyDecodingStrategy`를 `.convertFromSnakeCase`로 지정 후 디코딩을 수행합니다.

[코드]

```swift
struct Blog: Decodable {
    let name: String
    let postsCount: Int
    let imageUrl: URL
    let views: Int
    let tagLine: String

}

let json = """
{
    "name": "tommy's blog",
    "posts_count": 20,
    "image_url": "https://jusung.github.io",
    "views": 1000,
    "tag_line": "Happy Swift Coding"
}
""".data(using: .utf8)!

let decoder = JSONDecoder()  
decoder.keyDecodingStrategy = .convertFromSnakeCase // keyDecodingStrategy를 지정
let blog: Blog = try decoder.decode(Blog.self, from: json)
```
  
[실행결과]

```
Blog(name: "tommy\'s blog", postsCount: 20, imageUrl: https://jusung.github.io, views: 1000, tagLine: "Happy Swift Coding")
```

`decoder.keyDecodingStrategy`를 지정하면 `CodingKey` 선언 없이도 snake-case 데이터를 camel-case로 디코딩 가능한 것을 확인할 수 있습니다.

## 정리
이번 포스트에서는 “Codable을 사용해 json 데이터 디코딩 방법 3가지” 에 대해 살펴보았습니다.

서버에서 내려주는 json의 형태과 개수에 따라서 1, 2, 3의 방법 중에 하나를 적절히 선택해서 디코딩하면 됩니다.
특히 snake-case로 내려오는 경우에 디코더에 stratege를 지정해 사용하는 것을 추천드립니다.

👨🏻‍💻지식이 +1 늘었다.   
다음 포스트에서 또 만나요 🚀😄