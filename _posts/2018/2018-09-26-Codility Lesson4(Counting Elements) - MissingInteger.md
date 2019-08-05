---
layout: post
title: "[Codility] Lesson4 (Counting Elements) - MissingInteger"
category: 
  - Codility
comments: true
published: true
---

## 문제
This is a demo task.

Write a function:

`
public func solution(_ A : inout [Int]) -> Int
`

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.

Given A = [−1, −3], the function should return 1.

Write an efficient algorithm for the following assumptions:

- N is an integer within the range [1..100,000];
- each element of array A is an integer within the range [−1,000,000..1,000,000].

## 문제 이해
주어진 배열에서 존재하지 않는 가장 작은 양수를 찾는 문제입니다.

## 해결 전략
배열을 순회하며 원소가 양수인 경우 그 값을 `Set`변수에 저장합니다. 

배열의 순회를 마치고 만약 `Set`변수에 아무 값도 없다면 가장 작은 양수는 1이 됩니다.

배열을 순회하면서 `Set`변수에 순회중인 배열의 Index에 해당하는 값이 있는지 확인합니다. 만약 없으면 그 값이 최소 양수가 됩니다.

배열의 순회를 마치고 중간에 비어있는 값을 찾지 못한 경우 `배열 크기 + 1`, 다시말해 배열의 `마지막 원소 다음 숫자`가 가장 장은 숫자가 됩니다.

## 코드 (in Swift)
해결 전략을 코드로 구현하면 아래와 같습니다.

```swift
public func solution(_ A : inout [Int]) -> Int {
    var B = Set<Int>()
    
    for item in A {
        if item > 0 {
            B.insert(item)
        }
    }
    
    if B.count == 0 {
        return 1
    }
    
    for (index, _) in A.enumerated() {
        if B.contains(index + 1) == false {
            return index + 1
        }
    }
    
    return A.count + 1
}
```