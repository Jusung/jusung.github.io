---
layout: post
title: "[Xcode] Project, Workspace, Target, Configuration, Scheme"
tags: 
  - Xcode
comments: true
published: true
---

이번 포스트에서는 Xcode에서 빌드시 사용되는 기본 용어 Project, WorkSpace, Target, Configuration, Scheme 이 5가지에 대해 알아보도록 하겠습니다. 

iOS/macOS 앱 개발 경험이 있으신분이면 이 용어들에 대해 익숙하실텐데요. 정확히 알고 사용하신 분도 계실 것 같고 정확히 알지는 못하고 그냥 사용하신 분도 계실 것 같아서 정리해 봤습니다.

## 정의

### Project
Project는 제품을 빌드하기 위해 필요한 파일을 담고 있는 저장소입니다. 

여러 Target을 가질 수 있고 담을 수 있는 파일의 종류는 다음과 같습니다.

* Project 파일이 담을 수 있는 파일 종류 (각 파일에 대한 레퍼런스를 갖고 있습니다.)
	- 소스 파일 (header, implementation 파일 포함)
	- 라이브러리 및 프레임워크
	- 외부 리소스 파일
	- 이미지 파일
	- 인터페이스 빌더(nib) 파일

### WorkSpace
WorkSpace는 한개 이상의 프로젝트 파일을 담고 있는 저장소입니다.

![](https://lh3.googleusercontent.com/k_4dX6k_lmJh7qHxkj7UPNIcUNJ4UlefPZuNqb-YNK_XKyK90BTzBTW8EnKWRvsfTYPhGwZFuNdKhCNw7vs=w1000-no-tmp.jpg){: .center-image}
*SampleApp, Pods 2개의 프로젝트를 담고 있는 WorkSpace*
{: style="text-align: center;"}

### Target
Target은 프로젝트를 빌드하면 얻게 되는 최종 제품(End Product)에 대해 기술(Specified)돼 있는 녀셕입니다. 

얻을 수 있는 최종 제품의 종류로는 앱, 프레임워크, 정적 라이브러리, 유닛테스트 번들 등이 있습니다. 다시 말하면 "최종 제품을 어떤 설정을 갖고 어떤 형태로 빌드할래?" 라는 것에 대해 기술한 녀셕입니다.

Target에 기술되는 내용은 다음과 같습니다.

- 프로젝트를 빌드한 후 생성된 제품에 대한 참조(Reference)
- 제품을 빌드하는데 필요한 파일의 참조(Reference)
- 제품을 빌드하는데 필요한 설정 파일들(Configurations)

![](https://lh3.googleusercontent.com/o62659_JthSNuKnIAxtFluBOyVW1RSNVYeR3pNbeZNrUdokJ62fDEOtmxC_7vUoZLOzbQrr-hHK3P9LX8qA=w1000-no-tmp.jpg){: .center-image}
*Xcode에서 Target설정 화면*
{: style="text-align: center;"}


![](https://lh3.googleusercontent.com/dZYcMHswI4SSFFO69zH1ueTjVOHHLDQ-2x37sIVjOe-FUh4wWNf7cZ5ahDB2hOzPwr3ak03-dlLEzf5SJnE=w1000-no-tmp.jpg){: .center-image}
*Xcode에서 파일 추가시 볼 수 있는 새 파일을 어떤 타겟에 포함시킬지에 대한 옵션*
{: style="text-align: center;"}


![](https://lh3.googleusercontent.com/7a9PcVjhfT0mnVjdQTILeem25EILV2_pAQYkT_-MbSQB72fJv3bmK1G7ZbEq_dnRE7ttqDjDhlqkgB8zmn0=w1000-no-tmp.jpg){: .center-image}
*Xcode화면 우측 Inspector화면에서 볼 수 있는 특정 파일이 속한 타겟*
{: style="text-align: center;"}

### Configuration
빌드시 어떠한 프로세스로 어떤 빌드 변수를 사용해 빌드할 것인지 빌드 설정에 대해 명시한 녀석입니다.

프로젝트 생성시 보통 Debug와 Release 설정을 기본으로 제공해 줍니다. 원하는 설정을 추가할 수도 있습니다.

![](https://lh3.googleusercontent.com/fC046KlZR3OWh6nDfd18jJhjbs4us2-mrxBq39H9lUI92Qjv4_7DeD82xomoQqqrCyRZ_QhF6UQK0GA5GMs=w1000-no-tmp.jpg){: .center-image}
*Configuration에 Debug, Release에 QA와 Staging을 추가한 후 모습*
{: style="text-align: center;"}

### Scheme
Run, Test, Archive등의 명령을 실행했을 때, 프로젝트를 어떤 Target을 어떤 Configuration을 사용해 빌드 할 것인지 그리고 테스트는 어떻게 할 것인지 정의한 녀석입니다.

Xcode에서 Scheme을 관리하는 메뉴를 보신적 있으실 겁니다. 

![](https://lh3.googleusercontent.com/RAViIPZ7YjjnVx4tCzxAUARRgR23YIFQ8c5gXERuYm7ifm3aB-caE-jDv6jbrpdj9T7exSLegJLKLoNmiyM=w1000-no-tmp.jpg){: .center-image}
*Scheme에서 어떤 Target을 사용할지 선택*
{: style="text-align: center;"}

![](https://lh3.googleusercontent.com/j2MJ9OSQrRZCa97BTaxAevzLXwu1zDFJQkJuZ94QkyOkN90VfHT0NsjlC8KbHUH0McX2qeS1Fe825MP5g8Y=w1000-no-tmp.jpg){: .center-image}
*Run, Test, Profile, Analyze, Archive시 어떤 행동을 할지 정의*
{: style="text-align: center;"}

메뉴를 통해 Run, Test, Profile, Analyze, Archive를 선택할 수 있습니다.
![](https://lh3.googleusercontent.com/x1QQ89itDra-gv016O06fekKH6RPEf8nyclESpm-podKb1UZATZmzaBFJbGq1nNiEHPlm2MxooAZ6FONxi0=w1000-no-tmp.jpg){: .center-image}

다음은 Run실행시 Debug Configuration을 실행하도록 설정된 Scheme입니다.
![](https://lh3.googleusercontent.com/14Co9H4dIiqpOF1FiKlBePeVGwMWbv_nU8xk909MMZ1bctNUpNGkoqzd3fsh4BCvyqv-3M07TnT0FoBrN3k=w1000-no-tmp.jpg){: .center-image}

다음은 Archive실행시 Staging Configuration을 실행하도록 설정된 Scheme입니다.
![](https://lh3.googleusercontent.com/i__pYuuHOH0UuJz24vvVD5YzQfd2jVvlKkPG7ehluhFkydhYZJ82bMMDxyxHcDP-J9Byd49hA9lTldxlOjY=w1000-no-tmp.jpg){: .center-image}


Scheme의 이점은 Target과 Configuration을 일일이 변경하지 않고 Run, Test, Profile 등에 적절한 설정을 해두고 바로 실행할 수 있다는 것 입니다.

## Target, Scheme, Configuration의 관계
Target, Scheme, Configuration의 상관 관계를 도식화 하면 다음과 같습니다. 위에 설명한 내용을 잘 이해하셨다면 특별한 설명이 없어도 아래 그림이 이해되실 것입니다. 😅

![](https://lh3.googleusercontent.com/ILijnGVwysneDuAx_fvFW-HeUPl8lwAa9xkLp__vZm27unmT-B4sb4UJP1kpiwYObeKID2HZtE58dJYWHAg=w1000-no-tmp.jpg){: .center-image}
*Scheme 설정의 예*
{: style="text-align: center;"}

이상 Xcode에서 빌드시 자주 보게되는 Project, WorkSpace, Target, Configuration, Scheme 이 5가지에 대해 알아 보았습니다. 이제 보다 자신감을 갖고 Xcode빌드를 다룰수 있겠죠? 👍 

다음에 또 만나요~ 😄

## 참 고
* [Xcode Project](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Projects.html#//apple_ref/doc/uid/TP40009328-CH5-SW1)
* [Xcode Workspace](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Workspace.html#//apple_ref/doc/uid/TP40009328-CH7-SW1)
* [Xcode Target](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Targets.html#//apple_ref/doc/uid/TP40009328-CH4-SW1)
* [Xcode Scheme](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Schemes.html#//apple_ref/doc/uid/TP40009328-CH8-SW1)
* [StackOverflow - Xcode: What is a target and scheme in plain language?](https://stackoverflow.com/questions/20637435/xcode-what-is-a-target-and-scheme-in-plain-language#)