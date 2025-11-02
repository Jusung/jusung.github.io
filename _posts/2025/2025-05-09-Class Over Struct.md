---
layout: post
title: "SwiftData에서 모델링 시 왜 struct가 아닌 class를 사용할까?"
category:
  - SwiftUI
tags:
  - SwiftData
comments: true
published: true
---

SwiftData에서 모델링은 왜 `struct`를 사용하지 않고 `class`를 사용할까요? 
간단하게 정리하면 SwiftData 모델은 데이터의 동일성과 상태 추적을 위해 `class`를 사용합니다.
자세한 내용은 아래와 같습니다.

## **SwiftData에서 모델링에 `struct`가 아닌 `class`를 사용하는 이유**

**1. 한 곳에서 데이터를 변경하면 변경이 데이터가 사용되는 모든 곳에 반영되도록 하기 위해**

- 클래스를 사용하면 뷰가 데이터가 어디에서 왔는지, 데이터를 어디로 보내야하는지, 데이터를 동기화 해야하는지 알 필요 없음 (값 타입이 아니라 참조를 사용하기 때문)
- 구조체(값 타입)를 사용하는 경우 데이터를 받은 뷰가 변경된 데이터를 원래 위치로 다시보내거나, 데이터를 소유자가 보냈던 데이터를 뷰로 다시 가져와야 함.
- `ObservableObject` 과 `@Observable` 타입도 같은 이유로 클래스를 사용함 

﻿

**2. 모델은 다른 모델과 관계(relationship)를 가짐**

- 만약 구조체(값 타입)를 사용하게 되면 어떤 모델을 가져올 때 관계도 값으로 같이 복사해서 가져와야 함
  - (예) `Employee` 구조체가 `Manager`혹은 다른 `Empolyee`와 연결돼 있다면 `Employee` 하나를 가져오기 위해 연결된 `Manager`나 `Manager`의 다른 관계를 전부 복사해야 할 수도 있음
  - 관계가 얽인 구조체 하나를 완성하려면, 전체 데이터베이스를 복사해서 스냅샷처럼 만들어야할 수도 있음

## 참고

- [Why Swift Data models are classes?](https://www.reddit.com/r/swift/comments/1jhshjc/comment/mj9vxx0/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
- [Why are SwiftData models created as classes?](https://gitminam.com/1f5ebaa8982b80aa98a2e8f5e016d993?pvs=25)