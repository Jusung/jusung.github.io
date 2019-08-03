---
layout: post
title: "[Swift] 순수함수(Pure function)란?"
category:
  - Swift
tag:
  - managing state & object
comments: true
published: true
---

순수함수란 **외부 상태에 의존적이지 않고, 어떠한 사이드이펙트도 발생시키지 않는 함수**입니다.. 

순수함수는 언제 얼마나 많이 호출해도 항상 같은 `input`에 대해 같은 `output`을 반환합니다. 순수함수라는 말이 이론적인 컨셉으로 보이지만 실제 다음과 같은 실용적인 이점이 있습니다.

**이점**

	- 테스트가 용의하고, 재사용성이 올라감
	- 예측가능성이 높아짐

예제를 통해 더 자세히 알아보죠.

## 예제 1 : 함수 내부에서 값을 변경하는 함수
아래 함수는 `String`에 확장자가 없으면 추가하는 함수로 아직 완전히 순수하지 않은 함수입니다.
```swift
extension String {
    mutating func addSuffixIfNeeded(_ suffix: String) {
        guard !hasSuffix(suffix) else {
            return
        }
        append(suffix)
    }
}
```

이 함수는 함수 안에서 값을 변경하고 있는데, 이게 큰 문제가 아닌 것 같지만 `String`이  `Value Type`이라 나중에 문제가 될 수 있습니다.

이 함수는 다음과 같이 사용할 수 있습니다.
```swift
var fileName = contentName
fileName.addSuffixIfNeeded(".md")
try save(content, inFileNamed: fileName)
```

앞의 함수를 순수함수로 만들기 위해 함수 안에서 값을 변경시키는 대신 값을 반환하도록 다음과 같이 변경 가능합니다.
```swift
extension String {
    func addingSuffixIfNeeded(_ suffix: String) -> String {
        guard !hasSuffix(suffix) else {
            return self
        }
        
        return appending(suffix)
    }
}
```

코드를 조금만 변경했는데, 이 변경을 통해 **함수 내에서 상태를 변경 하던 부분을 제거**했습니다. 변경한 함수는 아래와 같이 사용가능합니다.
```swift
let fileName = contentName.addingSuffixIfNeeded(".md")
try save(content, inFileName: fileName)
```

또 다른 예제를 살펴보죠.

## 예제 2 : 외부 상태에 의존적인 함수
로그인 실패시 보여줄 메시지를 만드는 함수를 예를 들어보죠. 이 함수는 **로그인 실패 횟수에 따라 보여주는 메시지가 다릅니다.**
```swift
extension LoginController {
    func makeFailureHelpText() -> String {
        guard numberofAttempts < 3 else {
            return "Still can't log you in. Forgot your password?"
        }
        
        return "Invalid username/password. Please try again."
    }
}
```

코드에서 보다시피 외부 프로퍼티인  `numberofAttempts ` 값에 따라 반환하는 메시지가 다릅니다. 

이 함수를 순수함수로 만들기 위해 다음과 같이 상태 파라미터에 의존적인 함수로 변경할 수 있습니다.
```swift
extension LoginController {
    func makeFailureHelpText(numberofAttempts: Int) -> String {
        guard numberofAttempts < 3 else {
            return "Still can't log you in. Forgot your password?"
        }
        
        return "Invalid username/password. Please try again."
    }
}
```

앞에서 언급했던 것 처럼 순수함수의 가장 큰 이점은 테스트 하기가 쉽다는 것입니다.
`Input` 값을 넣으면 `Output` 값이 나옵니다.

방금 만들었던 함수는 다음과 같이 테스트가 가능합니다.
```swift
class LoginControllerTests: XCTestCase {
    func testHelpTextForFailedLogin() {
        let controller = LoginController()
        
        XCTAssertEqual {
            controller.makeFailureHelpText(numberOfAttemps: 0), 
            "Invalid username/password. Please try again."
        }
        
        XCTAssertEqual {
            controller.makeFailureHelpText(numberOfAttemps: 0), 
            "Still can't log you in. Forgot your password?"
        }
    }
}
```
인자 값 `numberOfAttemps`를 달리 넣는 것으로 쉽게 테스트 가능합니다.
이 밖에 순수함수는 다른 것에 의존적이지 않고 영향을 끼치지 않기 때문에 구조화 하기가 더 쉽습니다.

## 결론
코딩을 할때 앱 전체를 순수함수로만 구성하는 것은 매우 어렵습니다. 하지만 그렇게 하기만 하면 핵심 로직을 더 단단하게 만들고 테스트하기 쉽게 만들 수 있습니다.


## 참고
이 포스트는 외부 포스트를 요약한 것입니다. 더 자세한 내용을 확인하고 싶으신 분은 원문을 참조하세요.

**원문** : [Pure functions in Swift - by John Sundell](https://www.swiftbysundell.com/posts/pure-functions-in-swift)
