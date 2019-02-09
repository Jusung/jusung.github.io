---
layout: post
title: "[Codility] Lesson4 (Counting Elements) - PermCheck"
tags: 
  - Codility
comments: true
published: true
---

## 문제
A non-empty array A consisting of N integers is given.

A permutation is a sequence containing each element from 1 to N once, and only once.

For example, array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    A[3] = 2
    
is a permutation, but array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    
is not a permutation, because value 2 is missing.

The goal is to check whether array A is a permutation.

Write a function:

`
public func solution(_ A : inout [Int]) -> Int
`

that, given an array A, returns 1 if array A is a permutation and 0 if it is not.

For example, given array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    A[3] = 2
    
the function should return 1.

Given array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    
the function should return 0.

Write an **efficient** algorithm for the following assumptions:

- N is an integer within the range [1..100,000];
- each element of array A is an integer within the range [1..1,000,000,000].

## 문제 이해
주어진 배열 A가 순열인지 아닌지 확인하는 함수를 계산하는 문제입니다. 여기서 순열이란 N개의 정수로 구성된 배열 A에서 1에서 N까지 단 한번만 배열 A에 존재하는 경우 A를 순열이라 부릅니다.

## 해결 전략
어떤 배열이 순열임을 만족시키는 조건 2개를 문제로부터 유추 할 수 있습니다. 하나는 1에서 N까지 모든 수가 존재해야 하고, 다른 하나는 각 원소가 단 1번만 존재해야 한다는 것입니다.

첫번째 조건을 만족시키기 위해서 1부터 N까지의 합을 이용합니다. `N * (N + 1) / 2`를 하면 1부터 N까지 원소의 기대되는 합을 구할 수 있습니다. 배열의 각 원소를 더했을 때, 서로의 합이 일치하면 일단 첫번째 조건은 만족하는 것입니다.

하지만 합만 같아서는 배열 A에 단 한번만 존재한다는 조건을 확인할 수 없습니다. 같은 수가 적절히 존재하면 합은 같이 나올 수 있기 때문입니다. 그래서 이 조건을 확인하기 위해서 `Set(HashMap)`을 선언해서 배열의 각 원소를 순회하며 이 Set에 원소를 넣습니다. 원소를 넣기 전에 Set에 현재 순회하는 원소가 존재하는지 확인합니다. 존재하는 경우 이전에 원소를 넣은 것이기 때문에 중복값이 발견된 것입니다. 이 시점에서 바로 순열이 아님을 판별할 수 있기 때문에 순열이 아님을 나타내는 0을 반환합니다.


## 코드 (in Swift)
위 해결 전략을 코드로 구현하면 다음과 같습니다. 쉽죠? 😄

```swift
public func solution(_ A : inout [Int]) -> Int {
    var countCheckSet = Set<Int>()
 
    let N = A.count
    let expectedSum = N * (N + 1) / 2
    var sum = 0
 
    for element in A {
        if element > N {
            return 0
        }
 
        // found duplication
        if countCheckSet.contains(element) {
            return 0
        }
        else {
            countCheckSet.insert(element)
        }
 
        sum += element
    }
 
    if expectedSum == sum {
        return 1
    }
 
    return 0
}
```