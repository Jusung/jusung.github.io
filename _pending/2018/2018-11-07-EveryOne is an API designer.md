---
layout: post
title: "[요약] EveryOne is an API designer - John Sundell"
tags: 
  - 요약
comments: true
published: true
---

제가 좋아하는 [*John Sundell*](https://www.swiftbysundell.com) 이라는 Swift 개발자가 NSSpain 2017에서 발표한 "EveryOne is an API designer” 이라는 프리젠테이션의 요약입니다. 원본 영상은 [이곳](https://vimeo.com/234961067)에서 보실 수 있습니다.

요약은 영어와 한국어 2개로 해 두었습니다. 

![](https://lh3.googleusercontent.com/Hd2f1cDRptG3PPC9OXQqD_Rv9y9zQ8ySDqXW0gs9eSTrjZZ8z7PBem9RB3MW9EScpnOTmEcPpWFantEZTzA=w5000-no-tmp.jpg){: .center-image}

![](https://lh3.googleusercontent.com/4MdUziN_6Ufx-7OGiuEItW7MfR-fohvBx8Y0z-AWIoh8_-xsA4DSNSg9DXT1aEWTcNd5fzVZbwY3xfi8-MA=w5000-no-tmp.jpg){: .center-image}

[English]

## 3 API Characteristics

### 1. Contained

Great APIs are **contained**

* they don’t expose their **implementations details**
* which **removes confusion** and reduces the risk of “invalid use”
* **Protocols** are a great way to define contained APIs
* that can have their underlying implementation **easily changed.**

### 2. Conversational

Great APIs are **conversational**

* they make their **intent clear** by using readable declarations
* which **eliminates cruft** and makes usages more obvious.
* By defining **separate APIs** for separate actions & functionality
* we can make them **easier to read**, and **harder to misunderstand**

### 3. Scalable

Great APIs are **scaleable**

* they are **easy to start** using
* yet **flexible enough** for more advanced use cases.
* This can be achieved using **extensions** & **default arguments**.
* and by **moving overrides** from the call site to type definitions

---
[한국어]

## 좋은 API의 3가지 구성요소
### 1. 내장성

* 꼭 필요한 부분만 외부에 노출시키고 구현의 세부 내용을 외부에 노출하지 않음으로써, API의 잘못된 사용의 위험을 줄이고 모호함을 제거할 수 있습니다.
* 프로토콜(Protocol)을 이용하면 구현부를 쉽게 변경할 수 있으면서 좋은 내장성을 갖춘 API를 정의할 수 있습니다.

### 2. 읽고, 사용하기 쉬움
* 읽을 수 있는 선언을 통해 의도를 분명히 함으로써 복잡성을 제거하고 API를 보다 명확히 사용할 수 있습니다.
* 다른 행동과 함수에 대해 다른 API를 선언함으로써 더 읽기 쉽고 오해하기 힘든 API를 정의할 수 있습니다.

### 3. 확장성
* 좋은 API는 쉽게 사용할 수 있으면서도 고급 사용에 대해서도 유연하게 대응할 수 있습니다.
* 이런 확장성은 익스텐션(extension)과 기본 인자(default arguments) 그리고 오버라이딩을 함수의 호출에서 타입 정의로 이동시킴으로써 확보할 수 있습니다.