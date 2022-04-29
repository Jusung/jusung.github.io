---
layout: post
title: "[Xcode] 무선(Wireless) 빌드 사용하기"
category:
  - iOS
tag:
  - Xcode
comments: true
published: true
---

Xcode9 부터 맥과 기기(iPhone, iPad 등)를 케이블로 연결하지 않고, 기기에 바로 빌드 할 수 있는 기능이 추가되었습니다. 이 포스트에서는 이 기능을 사용하는 방법에 대해 알아보겠습니다.

이 기능을 사용하기 위해서는 **Xcode9 이상** , 디바이스가 **iOS11 이상** 이어야합니다.  

### 기능 설정하기
1. Xcode에서 프로젝트를 엽니다.

2. 아이폰과 맥을 USB 케이블로 연결합니다. (무선 빌드 설정을 위해 유선 연결이 한번 필요합니다. 😄)

3. Xcode의 Window -\> Devices and Simulators 메뉴를 선택합니다.

![]({{ site.baseurl }}/images/2018/wireless_build1.png)

4. 왼쪽 창에서 USB 케이블로 연결 된 아이폰을 선택하면 오른쪽에 현재 아이폰 정보가 보이면서 **Connect via network** 체크박스 항목이 선택되지 않은 상태인 것을 확인할 수 있습니다. 이 체크박스를 선택해 줍니다.

![]({{ site.baseurl }}/images/2018/wireless_build2.png)

5. Xcode에서 설정이 완료되면 왼쪽 창 연결된 아이폰 항목의 오른쪽에 지구본 모양의 아이콘을 확인할 수 있습니다.

![]({{ site.baseurl }}/images/2018/wireless_build3.png)

6. 설정이 완료 됐으니 맥과 아이폰간 USB 연결을 해지 합니다.

7. Xcode로 돌아가 Run 버튼을 클릭 합니다.

![]({{ site.baseurl }}/images/2018/wireless_build4.png)

👏 이제 무선으로 빌드를 더 편하게 할 수 있습니다. 😎

<object data="{{ site.baseurl }}/images/2022/11.pdf" width="300" height="300" type='application/pdf'> </object>