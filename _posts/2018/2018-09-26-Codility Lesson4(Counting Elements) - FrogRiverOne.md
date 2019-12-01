---
layout: post
title: "[Codility] Lesson4 (Counting Elements) - FrogRiverOne"
category: 
  - Algorithm
tag:
  - Codility
comments: true
published: true
---

## 문제
A small frog wants to get to the other side of a river. The frog is initially located on one bank of the river (position 0) and wants to get to the opposite bank (position X+1). Leaves fall from a tree onto the surface of the river.

You are given an array A consisting of N integers representing the falling leaves. A[K] represents the position where one leaf falls at time K, measured in seconds.

The goal is to find the earliest time when the frog can jump to the other side of the river. The frog can cross only when leaves appear at every position across the river from 1 to X (that is, we want to find the earliest moment when all the positions from 1 to X are covered by leaves). You may assume that the speed of the current in the river is negligibly small, i.e. the leaves do not change their positions once they fall in the river.

For example, you are given integer X = 5 and array A such that:

```
  A[0] = 1
  A[1] = 3
  A[2] = 1
  A[3] = 4
  A[4] = 2
  A[5] = 3
  A[6] = 5
  A[7] = 4
```

In second 6, a leaf falls into position 5. This is the earliest time when leaves appear in every position across the river.

Write a function:

`
public func solution(_ X : Int, _ A : inout [Int]) -> Int
`

that, given a non-empty array A consisting of N integers and integer X, returns the earliest time when the frog can jump to the other side of the river.

If the frog is never able to jump to the other side of the river, the function should return −1.

For example, given X = 5 and array A such that:

```
  A[0] = 1
  A[1] = 3
  A[2] = 1
  A[3] = 4
  A[4] = 2
  A[5] = 3
  A[6] = 5
  A[7] = 4
```
  
the function should return 6, as explained above.

Write an **efficient** algorithm for the following assumptions:

- N and X are integers within the range [1..100,000];
- each element of array A is an integer within the range [1..X].

## 문제 이해
개구리가 한 곳에서 다른 곳으로 이동할 수 있는 최소시간을 찾는 문제입니다.

## 해결 전략
1에서 N으로 이동하기 위해서는 1과 N을 포함에 두 수 사이에 나뭇잎이 모두 준비돼 있어야 합니다. 문제 해결을 위해서는 1에서 N까지의 모든 나뭇잎이 떨어져 개구리가 이동할 수 있도록 준비되는 최소시간을 계산하면 됩니다.

우선 배열 A의 모든 원소를 `Set`에 저장해 둡니다. 배열 A의 원소는 중복될 수 있지만 `Set`은 값의 중복을 허용하지 않기 때문에 A의 각 원소가 종류별로 단 한번만 저장돼 있습니다. 이후 배열 A를 순회하면서 현재 원소를 `Set`에서 제거하면 `Set`에 원소가 하나도 남아있지 않는 시점이 **개구리가 건너편으로 이동에 필요한 모든길이 준비된 시점** 이라고 말할 수 있습니다.

## 코드 (in Swift)
해결 전략을 코드로 구현하면 아래와 같습니다.

```swift
public func solution(_ X : Int, _ A : inout [Int]) -> Int {
    var existanceCheckSet = Set<Int>()
 
    for i in 1...X {
        existanceCheckSet.insert(i)
    }
 
    for (index, element) in A.enumerated() {
        existanceCheckSet.remove(element)
 
        if existanceCheckSet.count == 0 {
            return index
        }
    }
 
    return -1
}
```