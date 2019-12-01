---
layout: post
title: "[Codility] Lesson3 (Time Complexity) - PermMissingElem"
category: 
  - Algorithm
tag:
  - Codility
comments: true
published: true
---

## 문제
An array A consisting of N different integers is given. The array contains integers in the range [1..(N + 1)], which means that exactly one element is missing.

Your goal is to find that missing element.

Write a function:

`
public func solution(_ A : inout [Int]) -> Int
`

that, given an array A, returns the value of the missing element.

For example, given array A such that:

```
  A[0] = 2
  A[1] = 3
  A[2] = 1
  A[3] = 5
```

the function should return 4, as it is the missing element.

Write an efficient algorithm for the following assumptions:

- N is an integer within the range [0..100,000];
- the elements of A are all distinct;
- each element of array A is an integer within the range [1..(N + 1)].

## 문제 이해
정수 N개를 갖는 배열 A가 주어 졌을 때, A는 원소로 1에서 N+1사이의 정수를 갖는데, 딱 하나의 정수가 빠져있게 됩니다. 이 빠진 원소를 찾는 문제입니다.


## 해결 전략
N이 주어졌을 때 배열 A가 가져야 하는 숫자의 범위는 1~N+1로 정해져 있습니다. 그렇다는 의미는 N이 주어졌을때 A가 가져야 하는 정수들의 합도 정해져 있다는 뜻입니다. 그래서 N을 이용해 A가 원소로 갖을 모든 정수의 합을 계산하고 A배열을 순회하면서 계산된 정수의 합에서 순회하는 값을 빼주면 A가 갖기를 기대하지만 A에 포함돼 있지 않은 수만 남게 됩니다.

## 코드 (in Swift)
```swift
public func solution(_ A : inout [Int]) -> Int {
    var sum = 0
    
    let N = A.count
    let totalSum = (N + 1) * (N + 2) / 2  // N이 주어 졌을때 기대되는 전체 정수의 합을 구해줍니다. 보통 1~10까지의 합을 구하려면 (N+(N+1)) / 2로 계산할 수 있습니다.
    
    for element in A { // 현재 배열의 각 원소의 합을 구합니다.
        sum += element
    }
    
    return totalSum - sum  // 기대되는 원소의 합에서 실제 원소의 합을 빼면 비어있는 정수를 구할 수 있습니다.
}
```