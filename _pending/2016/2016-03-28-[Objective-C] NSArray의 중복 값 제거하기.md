---
layout: post
title: "[Objectivec] NSArray의 중복 값 제거하기"
tags: 
  - Objective-C
comments: true
published: true
---

`NSArray`에 중복 값이 존재하고 그것을 제거하고 싶을 때, 가장 쉬운 접근 방법은 for loop을 이용한 방법이 있을 것입니다.

하지만 이것보다 더 간단한 방법이 있습니다.

`NSOrderedSet`을 이용하는 것인데요. 이 클래스가 자동으로 `NSArray` 배열 내의 중복 값을 제거해 줍니다.

이 클래스를 이용해서 중복을 제거하는 코드는 아래와 같습니다.

```objc
NSOrderedSet *orderedSet = [NSOrderedSet orderedSetWithArray:yourArray];
NSArray *uniqueArray = [orderedSet array];
```

만약 배열에 정말 많은 데이터가 저장되어 있고 퍼포먼스를 고려해야 하는 작업이라면 반드시 직접 중복 제거 코드를 작성해야 하지만, 저장된 데이터가 몇 개 되지 않고 퍼포먼스를 고려하지 않아도 된다면 위와 같이 간편한 방법을 쓰지 않을 이유는 없을 것 같습니다.

이상 팁이었습니다.