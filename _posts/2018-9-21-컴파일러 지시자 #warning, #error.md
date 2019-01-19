---
layout: post
title: "[Swift 4.2] 컴파일러 지시자 #warning, #error"
tags: 
  - Swift
comments: true
published: true
---

## TODO와 FIXME
개발을 하다보면 아직 마무리가 안된 코드를 남겨 두고 다른 코드를 작업하는 경우가 종종 있습니다. 수정이나 추가가 필요한 코드를 발견 했지만 일단 현재 작업에 집중하기 위해 그 코드를 남겨두는 것이죠. 이럴 때 Swift 4.2 이전의 Xcode에서는 `//TODO:`나 `//FIXME:` 주석을 사용했습니다. 그래서 자신 혹은 다른 팀원에게 코드의 특정 부분에 추가적인 작업이 필요하다고 알릴 수 있었습니다.

```swift
func computeArray(xs: [Int], f: (Int) -> Int) -> [Int] {
        // TODO: Need to convert to Generic version of this function
        var result = [Int]()

        for x in xs {
            result.append(f(x))
        }
    
        return result
    }
    
func genericComputeArray<U>(xs: [Int], f: (Int) -> U) -> [U] {
    // FIXME: - Leaking Memory
    
    var result = [U]()
    
    for x in xs {
        result.append(f(x))
    }
    
    return result
}
```

이렇게 작성된 주석은 View -> Standard Editor -> Show Document Items 메뉴에서 트리 형태로 확인할 수 있었습니다. 그래서 어떤 부분에 추가적인 작업이 필요한지 파악할 수 있는 것이죠. 

![](https://farm2.staticflickr.com/1860/30928990868_e0e9e25249_b.jpg){: .center-image}

그런데 이 방법에는 큰 단점이 있습니다. 그것은 바로 이렇게 표시를 해 놓아도 빌드에는 아무런 영향을 끼치지 않는다는 것입니다. 다시 말하면 어딘가 코드를 수정해야한다고 표시는 해 두었지만, 주의를 기울여서 수정하지 않는다면 무엇인가 문제가 있는 코드가 그대로 배포될 수 있다는 것입니다. 

이 문제를 해결하기 위해 Xcode에서 build script를 추가해 `TODO`나 `FIXME`키워드를 찾아서 이 키워드가 있는 경우 컴파일러에게 경고를 뱉으라고 지정할 수 있었습니다. 현재 그렇게 사용하시는분도 계실것 같습니다. 하지만 Swift 4.2에서는 이와 관련된 컴파일러 지시자인  `#warning`, `#error` 가 추가 되었기 때문에 더 이상 그런 스크립트를 추가할 필요가 없습니다.[^1] 😎

## Swift 4.2에서 추가된 컴파일러 지시자
Swift 4.2에서는 `#warning`, `#error` 라는 컴파일러 지시자가 추가 됐는데, 이 지시자를 사용하면 빌드시 컴파일러에게 경고 혹은 에러를 뱉으라고 지시할 수 있습니다. 문법은 다음과 같이 단순 합니다.

```
#warning("message")
#error("message")
```

이 지시어를 코드에 적으면 빌드시 Xcode혹은 CLI 빌드시 다음과 같은 경고 혹은 에러를 발생시킬 수 있습니다.

![](https://farm2.staticflickr.com/1848/30928990848_f294cea650_b.jpg){: .center-image}

이를 살짝 응용해 `#warning`, `#error`를 다음과 같이 라이브러리의 최소 버전 확인이나 API Key 정보 누락 등도 표시할 수 있습니다.

```swift
#if MYLIB_VERSION < 3 && os(macOS)
#error("MyLib versions < 3 are not supported on macOS")
#endif
```

```swift
enum APICredentials {
  #warning("fill in your API key below")
  static let key = ""
  #warning("fill in your API secret below")
  static let secret = ""
}
```

지금까지 Swift 4.2에서 추가된 컴파일러 지시어  `#warning`, `#error` 에 대하여 알아 보았습니다. 보다 생산적인 방법 으로 즐코하세요~ 😆

[^1]: [SE-0196 Compiler Diagnostic Directives](https://github.com/apple/swift-evolution/blob/master/proposals/0196-diagnostic-directives.md){:target="_blank"}