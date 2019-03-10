---
layout: post
title: "ABI stability란?"
category:
  - 5.0
comments: true
published: true
---

Swift 5.0에서 가장 중요한 피처는 ABI stability의 지원이라고 할 수 있습니다. <br/> 
이번 포스트에서는 이 ABI stability 대해 알아보도록 하겠습니다.

## ABI란? 

ABI라고 하면 뭔가 떠오르는게 있지 않으시나요? 그건 바로 모두가 잘 아는 API! <br/> 
네~ ABI는 API와 비슷합니다.

**API**는 Application Programming Interface의 약자로 프로그래밍시 코드에서 사용하는 인터페이스입니다. **ABI**는 Application Binary Interface의 약자로 바이너리 간 인터페이스입니다.

런타임에 Swift 프로그램 바이너리는 다른 라이브러리와 ABI를 통해 상호작용합니다. 운영체제와 앱, 앱과 라이브러리간 상호작용을 위해 ABI를 사용하게 됩니다.

![](https://lh3.googleusercontent.com/ds6nDyjygC-1IodGRreC6WHUDfPIY5yI_95Vis1w9ZWjZjRhpPmGRlq6uC1Nnog8TZ-iGNg5AWfLbrW2mf4=w1000-no-tmp.jpg){: .width="505px" height="162px" .center-image}
*OS-App-Library간 상호작용*
{: style="text-align: center;"}

API에는 보통 함수의 이름, 타입, 인자, 리턴 타입 등을 정의 하는데, ABI에는 함수를 어떻게 호출할지, 메모리에 데이터를 어떻게 표현할지, 메타데이터를 어디에 놓고 어떻게 접근할지 등을 정의합니다.

## ABI Stability란?

ABI가 안정화 됐다는 말은 앞으로 ABI가 변하지 않을 것이라는 보장입니다. 즉, 바이너리의 내부 처리 방식은 바뀔 수 있지만 인터페이스는 유지된다는 것입니다. 그래서 ABI stability가 지원되면 ABI가 지원된 버전의 Swift 컴파일러로 컴파일한 앱과 이후 업데이트 된 Swift 컴파일러로 컴파일된 라이브러리의 바이너리간 호환이 가능합니다.

Swift 5.0 미만의 Swift는 ABI stable 하지 않습니다. 그래서 각 바이너리 번들에는 고유의 Swift 동적 라이브러리(.dylib 파일)를 갖고 있고 이 라이브러리는 바이너리간 상호 작용을 위해 사용합니다.

![](https://lh3.googleusercontent.com/Dxy8Z21iHAxb1fw8mmEV9M_eLSmMDkNM5x5hFgQ7LhTSkSrQiqHxIkgIc7nHhMWP-eeTWnfVhzUV4EXivCI=w1000-no-tmp.jpg){: .width="326px" height="643px" .center-image}
*ipa파일 내부의 동적 라이브러리 파일*
{: style="text-align: center;"}

Swift 5.0부터 ABI가 안정화 됐고 이후 Swift 표준라이브러리를 iOS, 즉 운영체제단에 심으면 ABI는 Swift 5.0 이후 모든 버전에서 호환돼 돌아갈 것 입니다.

![](https://lh3.googleusercontent.com/hK3PcOKcM_dUQQ3m9V1cdPj3TZIlbMNyH2BPHxMxK-gPWrNAp0_ENmYllHLNByuA1CUElqCjL8FkXRDkWRM=w1000-no-tmp.jpg){: .width="274px" height="191px" .center-image}
*ABI 안정로 인한 호환성 보장*
{: style="text-align: center;"}



## 이 점
ABI가 안정화 되면서 얻게 되는 이점은 무엇일까요?

* **앱 번들 크기 축소** : 이제 Swift 표준 라이브러리를 앱마다 포함시킬 필요가 없기 때문에 이 라이브러리 크기 만큼 앱 용량을 줄일 수 있습니다.

* **소스 호환성** : Swift가 새로 나올때마다 마이그레이션 하느라 고생 하셨죠? 이제 구버전으로 작성된 Swift 코드를 새 컴파일러에서 컴파일할 수 있습니다. 즉, 새버전으로의 마이그레이션 고통을 줄여줍니다. 😆

* **Swift 언어의 변경 축소** : ABI를 유지하기 위해서는 Swift 언어에서 함수의 인터페이스를 바꾸면 안됩니다. 리턴 타입, 매개변수, 숫자, 타입 등 자료형 정의나 구조체 정의, 상수 정의 등도 바뀌면 안됩니다. 그렇기 때문에 앞으로 Swift 언어가 보다 적게 변경될 것을 기대할 수 있습니다.

* **바이너리 프레임워크 & 런타임 호환성** : 바이너리로 된 프레임워크를 다양한 Swift 버전으로 배포하는 것이 가능해 집니다. 

## 제 한
앞으로 Swift는 ABI stability를 보장하기 위해 Swift 런타임이나 표준 라이브러리에 추가가 필요한 피처는 제안이 제한됩니다. 이 조건에 해당하는 것은 다음과 같습니다.

* 표준 라이브러리에 새로운 타입, 프로토콜, 프로토콜 순응(Conformance), 함수, 매소드, 프로퍼티 등을 추가하는 것
* Swift 타입시스템을 변경하는 것. 예를 들면, 새로운 타입을 추가하거나 기존 타입에 함수 에트리뷰트 같은 새로운 변경자를 추가하는 것 그리고 새로운 브릿징, 서브 타이핑, 동적 케스팅 관계를 추가하는 것

<br/>
Swift 5.0은 ABI Stability 지원으로 이제 언어의 완숙기에 접어들었다고 보이네요.<br/> 
이상으로 ABI Stabilibty에 대해 알아보았습니다. 😉

## 참 조

* [ABI Stability and More](https://swift.org/blog/abi-stability-and-more/)
* [Evolving Swift On Apple Platforms After ABI Stability](https://swift.org/blog/abi-stability-and-apple/)
* [ABI Dashboard](https://swift.org/abi-stability/)
* [Swift ABI Stability Manifesto](https://github.com/apple/swift/blob/master/docs/ABIStabilityManifesto.md)
* [Swift 5 ABI Stability](https://medium.com/swift-india/swift-5-abi-stability-769ccb986d79)
* [ABI란 무엇인가요?](https://www.slideshare.net/ssusere4785c/abi-34537158)
* [Wikipedia - Application binary interface]()
* [What is an application binary interface (ABI)?](https://stackoverflow.com/questions/2171177/what-is-an-application-binary-interface-abi)
* [x86 calling conventions](https://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_calling_conventions)