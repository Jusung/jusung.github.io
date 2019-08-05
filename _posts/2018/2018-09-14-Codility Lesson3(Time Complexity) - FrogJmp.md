---
layout: post
title: "[Codility] Lesson3 (Time Complexity) - FrogJmp"
category: 
  - Codility
comments: true
published: true
---

## 문제
A small frog wants to get to the other side of the road. The frog is currently located at position X and wants to get to a position greater than or equal to Y. The small frog always jumps a fixed distance, D.

Count the minimal number of jumps that the small frog must perform to reach its target.

Write a function:

`
public func solution(_ X : Int, _ Y : Int, _ D : Int) -> Int
`

that, given three integers X, Y and D, returns the minimal number of jumps from position X to a position equal to or greater than Y.

For example, given:

```
  X = 10
  Y = 85  
  D = 30
```
  
the function should return 3, because the frog will be positioned as follows:

- after the first jump, at position 10 + 30 = 40
- after the second jump, at position 10 + 30 + 30 = 70
- after the third jump, at position 10 + 30 + 30 + 30 = 100

Write an **efficient** algorithm for the following assumptions:

- X, Y and D are integers within the range [1..1,000,000,000];
- X ≤ Y.

## 문제 이해
시작위치 X에서 같거나 큰 위치 Y까지 몇번만에 이동할 수 있는지를 계산하는 문제입니다. 개구리가 한번에 이동할 수 있는 거리는 D로 주어집니다.

## 해결 전략
특별히 어려운 문제는 없어서 주어진 수를 가지고 적당히 빼기 나누기를 해주면 됩니다.

`(목표지점 - 시작지점) / 이동할 수 있는 거리` 계산을 해주면 원하는 값을 얻을 수 있겠네요.

## 코드 (in Swift)

```swift
public func solution(_ X : Int, _ Y : Int, _ D : Int) -> Int {
        let doubleX = Double(X)
        let doubleY = Double(Y)
        let doubleD = Double(D)

        return Int(ceil((doubleY - doubleX) / doubleD)) // 소수점은 ceil함수로 무조건 올림해줍니다.
    }
```