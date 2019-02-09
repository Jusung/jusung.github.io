---
layout: post
title: "[Codility] Lesson2 (Arrays) - CyclicRotation"
tags: 
  - Codility
comments: true
published: true
---

## 문제
An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

The goal is to rotate array A K times; that is, each element of A will be shifted to the right K times.

Write a function:

`
public func solution(_ A : inout [Int], _ K : Int) -> [Int]
`

that, given an array A consisting of N integers and an integer K, returns the array A rotated K times.

For example, given


    A = [3, 8, 9, 7, 6]
    K = 3


the function should return [9, 7, 6, 3, 8]. Three rotations were made:

    [3, 8, 9, 7, 6] -> [6, 3, 8, 9, 7]
    [6, 3, 8, 9, 7] -> [7, 6, 3, 8, 9]
    [7, 6, 3, 8, 9] -> [9, 7, 6, 3, 8]
    
For another example, given

    A = [0, 0, 0]
    K = 1
    
the function should return [0, 0, 0]

Given

    A = [1, 2, 3, 4]
    K = 4
    
the function should return [1, 2, 3, 4]

Assume that:

- N and K are integers within the range [0..100];
- each element of array A is an integer within the range [−1,000..1,000].

In your solution, focus on **correctness**. The performance of your solution will not be the focus of the assessment.


## 문제 이해
입력 값으로 정수 배열과 얼마나 우측으로 이동했는지를 나타내는 K를 입력 받고, 출력으로 주어진 배열에서 가장 오른쪽에 있는 원소를 오른쪽으로 K번 이동한 배열을 반환합니다.

## 해결 전략
이번 문제는 성능보다 올바른 동작(Correctness)에 초점에 맞춰져 있기 때문에 특별한 전략없이 그냥 주어진 기능을 수행하는 코드를 작성합니다.

## 코드 (in Swift)

```swift
public func solution(_ A : inout [Int], _ K : Int) -> [Int] {
        guard A.count > 0 && K > 0 else {
            return A
        }
     
        for _ in 0...(K - 1) { // 정수 배열을 순회합니다.
            let lastElement = A.removeLast()  // 배열에서 가장 오른쪽 원소를 꺼내고 배열에서는 제거합니다.
            A.insert(lastElement, at: 0) // 배열에서 꺼낸 원소를 배열의 가장 처음에 삽입합니다. 이 과정을 K번 반복합니다.
        }
     
        return A
    }
```