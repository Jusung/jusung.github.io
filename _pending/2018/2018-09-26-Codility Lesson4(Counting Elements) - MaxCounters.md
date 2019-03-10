---
layout: post
title: "[Codility] Lesson4 (Counting Elements) - MaxCounters"
tags: 
  - Codility
comments: true
published: true
---

## 문제
You are given N counters, initially set to 0, and you have two possible operations on them:

- increase(X) − counter X is increased by 1,
- max counter − all counters are set to the maximum value of any counter.

A non-empty array A of M integers is given. This array represents consecutive operations:

- if A[K] = X, such that 1 ≤ X ≤ N, then operation K is increase(X),
- if A[K] = N + 1 then operation K is max counter.

For example, given integer N = 5 and array A such that:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4

the values of the counters after each consecutive operation will be:

    (0, 0, 1, 0, 0)
    (0, 0, 1, 1, 0)
    (0, 0, 1, 2, 0)
    (2, 2, 2, 2, 2)
    (3, 2, 2, 2, 2)
    (3, 2, 2, 3, 2)
    (3, 2, 2, 4, 2)

The goal is to calculate the value of every counter after all operations.

Write a function:

`
public func solution(_ N : Int, _ A : inout [Int]) -> [Int]
`

that, given an integer N and a non-empty array A consisting of M integers, returns a sequence of integers representing the values of the counters.

Result array should be returned as an array of integers.

For example, given:

```
    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
```
    
the function should return [3, 2, 2, 4, 2], as explained above.

Write an **efficient** algorithm for the following assumptions:

- N and M are integers within the range [1..100,000];
- each element of array A is an integer within the range [1..N + 1].

## 문제 이해
배열의 값에 따라 해당 원소의 값을 1증가시키는 `increase(X)`연산을 수행하거나 모든 원소를 해당 값으로 바꾸는 `max counter`연산을 배열의 모든 원소 값을 순회하며 수행한 결과 최종 배열을 반환합니다.

수행할 연산을 결정하는 방법은 위 문제의 조건을 따릅니다.

## 해결 전략
문제해결의 핵심은 `max counter`연산시 모든 원소의 값을 변경해야 하는데 이 연산이 필요할때마다 모든 원소의 값을 변경하는 것이 아니라, 배열을 단 한번만 순회하면서 `max counter`연산의 처리를 가능한 후처리로 하는 것입니다. 만약 그렇지 않으면 돌아는 가지만 처리해야할 데이터가 많아지면 알고리즘 성능이 급격히 나빠지게 됩니다.

그래서 `max counter`연산을 수행해야 할때마다 그 연산의 값을 저장하고 있다가 `increase(X)`연산시 `max counter`연산을 후처리로 수행하고, 만약 이미 지나간 원소에 대해서는 배열의 순회가 끝나고 `max counter`연산을 수행해 max값을 맞춰 춥니다.

## 코드 (in Swift)
해결 전략을 코드로 구현하면 아래와 같습니다.

```swift
public func solution(_ N : Int, _ A : inout [Int]) -> [Int] {
    var operations = Array(repeating: 0, count: N)
    var nextMax = 0
    var curMax = 0
 
    for element in A {
        let operationIndex = element - 1
 
        if element < N + 1 {
            if operations[operationIndex] <= curMax {
                operations[operationIndex] = curMax
            }
 
            operations[operationIndex] += 1
 
            if operations[operationIndex] > nextMax {
                nextMax = operations[operationIndex]
            }
        }
        else {
            curMax = nextMax
        }
    }
 
    for index in 0..<operations.count {
        if operations[index] < curMax {
            operations[index] = curMax
        }
    }
 
    return operations
}
```