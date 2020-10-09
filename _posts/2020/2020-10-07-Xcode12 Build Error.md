---
layout: post
title: "[Xcode] Xcode12에서 시뮬레이터 빌드 오류 원인 및 해결방법"
category:
  - Xcode
tags:
  - Xcode 12.0
comments: true
published: true
---

이번 포스트에서는 Xcode12에서 시뮬레이터에 빌드시 오류가 발생하는 원인과 해결방법에 대해 알아 보겠습니다. 포스트가 제법 깁니다. 😅

간단히 알고 넘어가실 분은 TL;DR만 읽으셔도 됩니다.

## TL;DR

### 빌드 오류 원인

- Xcode12에서 ARM기반 맥(애플실리콘)을 지원함에 따라 아이폰 시뮬레이터에 ARM용 아키텍쳐 `arm64`가 추가돼 발생

### 해결 방법

Build Setting에

![]({{ site.baseurl }}/images/2020/Xcode Build Error0.png)

1. `EXCLUDED_ARCHS`에 arm64를 추가
2. `VALID_ARCHS` 제거

여기까지가 간단한 문제 원인과 해결방법입니다.
다음 내용부터는 보다 자세한 내용입니다.

‼️ 주의!
내용에 많은 소설과 뇌피셜이 포함돼 있습니다.
소설이라고 판단되는 부분은 알려주시면 감사드리겠습니다.
(저도 빌드관련해서 잘 몰라요. 😭)

## 문제 원인 자세히 살펴보기

문제의 원인을 살펴보기전에 우선 Xcode에서 빌드와 관련해 알아야 할 내용이 두 가지 있습니다.

첫째, 아이폰은 기종에 따라 각기 다른 아키텍쳐의 CPU를 사용한다.

	- arm64 : 아이폰 5S 및 이후 기종 (6, 6S, SE, 7…)
	- arm7 : 아이폰 5, 5C
	- arm7s : iOS7 지원 옛날 기기

![]({{ site.baseurl }}/images/2020/Xcode Build Error1.png)
*아이폰 기기별 사용 아키텍쳐*
{: style="text-align: center;"}

둘째, 빌드시 바이너리가 생성되는데, 이 바이너리는 각 CPU 아키텍쳐에 맞게 생성해야 한다. (아키텍쳐별로 기계어 명령어 셋 등이 다름)

이제 본격적으로 설명을 시작하겠습니다.

앱을 하나 만들었는데, 이 앱을 아이폰 5S에서부터 최신기기인 iPhone 11까지 동작하도록 하려면 어떻게 빌드해야 할까요?

이 조건을 만족시키기 위해서는 `arm64` 바이너리 하나만 생성하면 안되고 `arm64`, `arm7`, `arm7s` 바이너리를 각각 생성해 원하는 모든 기기에서 동작할 수 있도록 해야합니다.

Xcode에서 이와 관련해 설정하는 부분은 `Build Setting -> Valid Architectures` 에 있었습니다.

![]({{ site.baseurl }}/images/2020/Xcode Build Error2.png)
*Valid Architectures 설정(Xcode11)*
{: style="text-align: center;"}

이렇게 설정하면 빌드시 Valid Architectures로 지정한 모든 아키텍쳐 바이너리를 생성해 앱이 각 아키텍쳐를 사용하는 모든 기기에서 동작할수 있도록 할 수 있습니다.

여기서 질문! ✋

그렇다면 아이폰 시뮬레이터는 어떤 아키텍쳐를 사용할까요?
아이폰5 시뮬레이터면 `arm7`을 아이폰11시뮬레이터면 `arm64`를 사용할까요?

아닙니다!

아이폰 시뮬레이터는 맥에서 동작합니다. 그래서 아이폰 시뮬레이터는 맥 CPU의 아키텍쳐를 따릅니다. 요즘 맥은 core i5, i7 등의 인텔 CPU를 사용하고 있는데 이 CPU는 `x86_64`라는 아키텍쳐를 사용합니다. 그래서 시뮬레이터는 `x86_64` 아키텍쳐를 사용합니다.

다음 위치에 SKDSetting 파일에서 시뮬레이터 아키텍쳐 정보가 `x86_64`라는 것을 확인할 수 있습니다.

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/SDKSettings.json
```

![]({{ site.baseurl }}/images/2020/Xcode Build Error3.png)
*Xcode 11 시뮬레이터의 아키텍쳐 : x86_64*
{: style="text-align: center;"}

Xcode에 아이폰11 기기를 연결하고  debug 설정으로 Run을 실행하면 어떤 일이 발생할까요?

![]({{ site.baseurl }}/images/2020/Xcode Build Error4-0.png)

앞에서 살펴본대로 Valid Architecture에 지정한 `arm64`, `arm7`, `arm7s` 용 빌드를 생성해 기기에 심습니다.
그런데 여기서 한가지 의문이 생깁니다.

> 아이폰11은 `arm64` 아키텍쳐를 사용하는데, 아이폰 11에다가 빌드하면 `arm64` 바이너리만 만들면 되지 굳이 `arm7`, `arm7s` 바이너리까지 만들 필요가 있을까?

맞습니다. 이럴때 사용하기 위한 옵션이 `Build Active Architecture Only`입니다.

![]({{ site.baseurl }}/images/2020/Xcode Build Error5.png)

Build Setting의 이 옵션을 활성화 시키면 Xcode에서 빌드시 Valid Architectures를 참조하지 않고, **현재 연결된 기기를 감지해 그 기기에 맞는 아키텍쳐용 빌드만 생성**합니다. 다시말해 이 옵션을 활성화하고 아이폰11에 빌드를 하면 `arm64`용 바이너라만 빌드가 되는 것이죠.

![]({{ site.baseurl }}/images/2020/Xcode Build Error4.png)

시뮬레이터도 마찬가지 입니다. Run Destination을 시뮬레이션으로 선택해 Run을 수행하면 시뮬레이터 아키텍쳐를 감지해 해당하는 아키텍쳐 바이너리를 생성해 심습니다. 이 경우에는 `x86_64` 아키텍쳐 용 바이너리를 만듭니다.

이 옵션을 왜 debug 설정에서만 true로 설정해 사용하는지는 이제 이해가 되시죠?

여기까지가 Xcode12 이전에 빌드 수행시 기기 및 시뮬레이터에서 동작하던 방식입니다.
이제 Xcode12에서 달라진 동작에 대해 알아보겠습니다.

## Xcode12 에서 동작하는 방식

먼저 Xcode12의 [Release Note](https://developer.apple.com/documentation/xcode-release-notes/xcode-12-release-notes)를 살펴보겠습니다.

![]({{ site.baseurl }}/images/2020/Xcode Build Error6.png)
*Xcode12의 Release Note 내용의 일부*
{: style="text-align: center;"}


관련해서 얘기하고 있는 것은 크게 세 가지 입니다.

1. Build Setting에 `Valid Architectures(VALID_ARCHS)`가 더 이상 포함되지 않음 (이 설정을 사용하는 것을 장려하지 않음)
2.  Valid Architectures 설정 대신 `EXCLUDED_ARCHS` 설정이 생김
3. 기존 Valid Architectures 내용은 사용자 정의 섹션(User-Defined)에 노출됨

![]({{ site.baseurl }}/images/2020/Xcode Build Error7.png)
*`Valid Architectures(VALID_ARCHS)` 설정이 더 이상 `Architectures`섹션에 있지않고 `User-Defined`섹션에 노출된 것을 확인하실 수 있습니다*
{: style="text-align: center;"}

정리하자면 기존에는 빌드시 **원하는 아키텍쳐를 지정**하는 방식을 사용했는데,

Xcode12에서는 반대로 빌드시 **제외시킬 아키텍쳐를 지정**하는 방식을 사용(필수x, 권장하는 부분)하는 쪽으로 바꼈습니다.

Xcode12에서 왜 이렇게 변경한걸까요?

그 이유는 바로 앞으로 출시될 ARM기반 맥북 때문입니다.

애플에서 머지않아 ARM기반 맥이 출시됩니다. 그동안 맥은 인텔 CPU기반이었습니다.(더 거슬러 올라가면 PowerPC) 하지만 곧 ARM용 맥도 출시가 돼서 앞으로는 인텔 CPU 기반 맥과 ARM기반 맥이 공존하는 상황이 됐습니다.

앞서 언급드린것처럼 아이폰 시뮬레이터는 맥의 아키텍쳐를 따릅니다. 현재 아이폰 시뮬레이터의 아키텍쳐는 `x86_64`입니다. ARM기반 맥은 `arm64`구조를 사용합니다.

Xcode12는 인텔 CPU 맥에서 ARM용 맥으로 넘어가는 과도기에 있는 앱으로, 두개 아키텍쳐를 모두 지원합니다. 다시말해 현재 사용 중인 `x86_64` 아이폰 시뮬레이터를 지원하고 앞으로 나올 `arm64`도 지원합니다.

바로 이 지점에서 시뮬레이터 호환성 이슈가 발생합니다.

전에는 시뮬레이터를 선택 후 빌드를 하면 `x86_64`용 빌드만 했는데, Xcode 12에서 시뮬레이터는 `x86_64`, `arm64`를 다 지원하기때문에 `x86_64`용과, `arm64`용으로 빌드를 수행합니다. 이 과정에서 현재 맥은 `x86_64`용 맥이기 때문에 `arm64`용 빌드 수행시 에러가 발생합니다.

다음 위치에 SKDSetting 파일에 Xcode12용 시뮬레이터의 아키텍쳐 정보가 `x86_64`, `arm64`라는 것을 확인할 수 있습니다.

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/SDKSettings.json
```

![]({{ site.baseurl }}/images/2020/Xcode Build Error8.png)
*새로운 시뮬레이터의 아키텍쳐 : arm64, x86_64*
{: style="text-align: center;"}

빌드시 이 에러를 막기 위해서는 어떻게 해야할까요?
그렇습니다. 현재는 `arm64`용 맥북이 출시되지 않았기 때문에 시뮬레이터 빌드시 `arm6`4용 빌드를 진행하지 않도록 해야합니다.

이 설정을 Xcode12 에서 새롭게 추가된  `EXCLUDED_ARCHS`에 설정할 수 있습니다.

![]({{ site.baseurl }}/images/2020/Xcode Build Error9.png)
*EXCLUDED_ARCHS 변수를 설정하면 Xcode에서 Excluded Architectures 섹션에 노출됩니다.*
{: style="text-align: center;"}

Build Setting에 `EXCLUDED_ARCHS` 항목에

> 빌드를 시뮬레이터에 하는 경우에는 arm64 바이너리용 빌드는 하지 말아줘.

라고 지정을 하면 빌드시 `arm64` 빌드를 수행하지 않아 에러가 발생하지 않습니다.

이것이 이번 Xcode12에서 모든 아이폰 개발자를 힘들게 만든 원인 입니다.

## 해결하지 못한 내용

참고로 Xcode12 에서 시뮬레이터 빌드 오류를 수정하려면 기존에 `VALID_ARCH` 설정도 함께 지워야하는데, 이 부분은 애플에서 사용하지 않는 것이 좋다라고 언급한 내용 정도만 확인하고, 정확히 빌드오류 해결과 어떤 연관이 있는지는 확인하지 못하였습니다.

기존의 방식에 따르면 `BUILD ACTIVE ARCHITECTURE ONLY`를 세팅하면 위 설정은 참고하지 않는데… 왜 설정이 있다고 오류가 나는지는 잘 모르겠네요.
아시는분 알려주시면 감사드리겠습니다.

이상 Xcode12에서 시뮬레이터 빌드 오류 원인 및 해결방법에 대해 살펴봤습니다.

(👨🏻‍💻지식이 +10 늘었다. 이번 포스트는 지식 + 10 정도 되쥬? 인정이쥬? 🤣)

다음 포스트에서 또 만나요~ 🚀😄 (아마도 내년에~~)

**[참 고]**

- [CPU Architectures](https://docs.elementscompiler.com/Platforms/Cocoa/CpuArchitectures/)

- [Build Setting Reference](https://developer.apple.com/library/archive/documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html#//apple_ref/doc/uid/TP40003931-CH3-SW157)

- [Xcode Build Settings](https://xcodebuildsettings.com/#group-source-versioning)

- [Deep dive Xcode Build Settings](https://medium.com/macoclock/deep-drive-xcode-build-settings-827c3ce4811c)

- [Wrestling with Xcode 12 and ARM64](https://medium.com/@johny.urgiles/wrestling-with-xcode-12-and-arm64-6c7977922abb)

- [ARM-Wrestling Your iOS Simulator Builds – Weaponized Fluff](https://apontious.com/2020/08/23/arm-wrestling-your-ios-simulator-builds/)

- [iOS Devices: Releases, Firmware, Instruction Sets, Screen Sizes](https://www.innerfence.com/howto/apple-ios-devices-dates-versions-instruction-sets)

- [Xcode 12 beta 4 - error compiling](https://developer.apple.com/forums/thread/656509)

- [cocoapods - Xcode 12 build target in wrong order? - Stack Overflow](https://stackoverflow.com/questions/63391793/xcode-12-build-target-in-wrong-order)

- [Xcode 12, building for iOS Simulator, but linking in object file built for iOS, for architecture arm64 - Stack Overflow](https://stackoverflow.com/questions/63607158/xcode-12-building-for-ios-simulator-but-linking-in-object-file-built-for-ios)

- [What is Build Active Architecture Only](https://samwize.com/2015/01/14/what-is-build-active-architecture-only/)

- [https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)

- [Setting Build Active Architectures Only To YES Pros vs Cons?](https://stackoverflow.com/questions/27017523/setting-build-active-architectures-only-to-yes-pros-vs-cons)

- [cocoapods - Xcode 12 build target in wrong order? - Stack Overflow](https://stackoverflow.com/a/63405201)

- [ios - What’s the difference between “Architectures” and “Valid Architectures” in Xcode Build Settings? - Stack Overflow](https://stackoverflow.com/questions/12701188/whats-the-difference-between-architectures-and-valid-architectures-in-xcode)

- [https://developer.apple.com/library/archive/documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html#//apple_ref/doc/uid/TP40003931-CH3-SW59](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)

- [Xcode 12, building for iOS Simulator, but linking in object file built for iOS, for architecture arm64 - Stack Overflow](https://stackoverflow.com/questions/63607158/xcode-12-building-for-ios-simulator-but-linking-in-object-file-built-for-ios/64139830#64139830)

- [Xcode Build Active Architecture Only](https://useyourloaf.com/blog/xcode-build-active-architecture-only/)

- [https://pewpewthespells.com/blog/buildsettings.html#action](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)

- [Xcode Build Settings](https://xcodebuildsettings.com/#excluded_archs)

- [https://msolarana.netlify.app/2020/08/02/broken-builds-with-universal-xcode/](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)

- [Xcode 12GM Build issues](https://medium.com/@abhishekbedi/xcode-12gm-build-issues-sim-device-cracked-cce7c5620646)

- [https://stackoverflow.com/questions/63607158/xcode-12-building-for-ios-simulator-but-linking-in-object-file-built-for-ios](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)

- [What are the universal binary codes for Xcode? - Quora](https://www.quora.com/What-are-the-universal-binary-codes-for-Xcode)

- [https://help.apple.com/xcode/mac/8.0/#/itcaec37c2a6?sub=devf0a9d5aca](https://stackoverflow.com/questions/6151549/how-can-i-build-a-specific-architecture-using-xcodebuild)
