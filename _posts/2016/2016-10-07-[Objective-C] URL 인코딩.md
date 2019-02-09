---
layout: post
title: "[Objective-C] URL 인코딩"
tags: 
  - Objective-C
comments: true
published: true
---

File URL이나 HTTP URL을 만들 때, URL이 영어와 기본 문자로만 구성되어 있는 경우, 인코딩 없이 NSURL기본 매소드로 다음과 같이 URL을 생성할 수 있습니다.

`
NSURL *myURL = [NSURL URLWithString:@"http://kyejusung.com/wp-content/uploads/2011/01/OneStepWithFaith.jpg"];
`

만약 URL에 비영어 문자, 예를들면 한글 같은 것이 포함되어 있다면 인코딩을 해줘야 합니다. `stringByAddingPercentEncodingWithAllowedCharacters`를 사용하면 UTF-8을 이용하여 인코딩 합니다.

```objc
NSCharacterSet *allowedCharacterSet = [NSCharacterSet URLQueryAllowedCharacterSet];
NSURL *myURL = [NSURL URLWithString:[@"http://kyejusung.com/wp-content/uploads/2016/10/조선왕조실록-1권.jpg" stringByAddingPercentEncodingWithAllowedCharacters:allowedCharacterSet]];
```

인코딩 전 URL : http://kyejusung.com/wp-content/uploads/2016/10/조선왕조실록-1권.jpg
인코딩 후 URL : http://kyejusung.com/wp-content/uploads/2016/10/%EC%A1%B0%EC%84%A0%EC%99%95%EC%A1%B0%EC%8B%A4%EB%A1%9D-1%EA%B6%8C.jpg

만약 인코딩된 URL에서 파일 이름을 가져오려면 어떻게 해야할까요?

“http://kyejusung.com/wp-content/uploads/2016/10/%EC%A1%B0%EC%84%A0%EC%99%95%EC%A1%B0%EC%8B%A4%EB%A1%9D-1%EA%B6%8C.jpg”

이 URL에서 `“%EC%A1%B0%EC%84%A0%EC%99%95%EC%A1%B0%EC%8B%A4%EB%A1%9D-1%EA%B6%8C.jpg”` 이 부분이 파일 이름인데 디코딩을 해야합니다.
편리하게도 `NSURL`에서 자동으로 디코딩해서 파일이름을 넘겨주는 `lastPathComponent`라는 매소드를 제공해주고 있습니다.

> [lastPathComponent](https://developer.apple.com/reference/foundation/nsurl/1417444-lastpathcomponent){:target="_blank"}
> 
This property contains the last path component, unescaped using the replacingPercentEscapes(using:) method. For example, in the URL file:///path/to/file, the last path component is file.

매소드 이름만 보면 URL의 마지막 요소만 넘겨줄 것 같은데, 디코딩도 해주네요.

이상으로 열거된 API를 이용하면 URL인코딩과 디코딩을 쉽게 하실 수 있습니다.