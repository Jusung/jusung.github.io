---
layout: post
title: "[Codility] Lesson2 (Arrays) - OddOccurrencesInArray"
tags: 
  - Codility
comments: true
published: true
---

## 문제
A non-empty array A consisting of N integers is given. The array contains an odd number of elements, and each element of the array can be paired with another element that has the same value, except for one element that is left unpaired.

For example, in array A such that:

```
  A[0] = 9  A[1] = 3  A[2] = 9
  A[3] = 3  A[4] = 9  A[5] = 7
  A[6] = 9
```

- the elements at indexes 0 and 2 have value 9,
- the elements at indexes 1 and 3 have value 3,
- the elements at indexes 4 and 6 have value 9,
- the element at index 5 has value 7 and is unpaired.

Write a function:

`
public func solution(_ A : inout [Int]) -> Int
`

that, given an array A consisting of N integers fulfilling the above conditions, returns the value of the unpaired element.

For example, given array A such that:

```
  A[0] = 9  A[1] = 3  A[2] = 9
  A[3] = 3  A[4] = 9  A[5] = 7
  A[6] = 9
```
  
the function should return 7, as explained in the example above.

Write an **efficient** algorithm for the following assumptions:

- N is an odd integer within the range [1..1,000,000];
- each element of array A is an integer within the range [1..1,000,000,000];
- all but one of the values in A occur an even number of times.


## 문제 이해
홀수로 이루어진 배열에서 같은 원소가 짝을 이루지 않는 원소가 있습니다. 이 짝이 없는 원소를 찾아서 반환하면 됩니다. 

## 해결 전략
이번 문제는 성능에 초점이 맞춰져 있기 때문에 문제에 대한 정확한 동작이 수행되야함은 물론이고 성능도 좋아야 합니다.


## 코드 (in Swift)

### 처음 접근했던 방법
Set(HashMap)을 하나 선언합니다. Set은 값의 중복을 허용하지 않습니다. 그래서 입력한 배열의 원소를 순회하면서 특정 원소가 Set에 있으면 그 원소를 제거하고 없으면 원소를 Set에 넣는 방식으로 짝을 지었습니다.
다시말해, Set에 원소가 없으면 원소를 넣고 있으면 넣었던 원소를 제거하는 것이죠. 배열에 특정 원소가 짝수인 경우에는 Set에 그 원소가 없을 것이고 홀수인 경우에는 남아 있겠죠. 그리고 마지막에 Set에 남아있는 원소가 짝이 없는 원소가 될 것 입니다. 이런 논리를 코딩한 것이 아래와 같습니다.

```swift
public func solution(_ A : inout [Int]) -> Int {
    var temp = Set<Int>()
    
    for item in A {
        if temp.contains(item) {
            temp.remove(item)
        }
        else {
            temp.insert(item)
        }
        
    }
    
    if temp.count == 1 {
        return temp.first!
    }
    
    return 0
}
```
위 코드가 문제 해결을 위해 정확히 동작은 합니다. 하지만 배열이 커지면 성능이 문제에서 요구하는 만큼 좋지 않았습니다. 그래서 고민고민 했는데 결국 스스로 방법을 찾지는 못하였고 구글링을 했습니다. 😅

### 구글링해서 찾은 방법
찾은 방법 중 하나가 비트연산자 중 XOR 연산자를 사용하는 것입니다. XOR 연산은 이진수 두개를 연산해서 각 자리의 값이 같으면 0, 다르면 1을 반환합니다. 짝을 이룬다는 것은 값이 같다는 것 입니다. 값이 같은 경우에는 XOR 연산에서 0으로 무시되기 때문에 짝을 이루지 않는 원소만 남게 됩니다. 이 연산은 끝가지 수행만 되면 순서는 상관없습니다. 이 XOR 연산을 이용해 짝이 없는 원소를 쉽게 찾아낼 수 있습니다. 

이 방법은 검색을 해보지 않았으면 제 머리에서는 생각해 내지 못했을 방법이었네요. 코드는 아래와 같이 매우 단순합니다. 코딩을 하면서 `+=`, `-=` 등을 써봤지만 `^=`는 처음 써봤네요. ㅋㅋㅋ 거의 10년 코딩 인생에서 ㅎㅎ. 

```swift
public func solution(_ A : inout [Int]) -> Int {
    var temp = 0
    
    for item in A {
        temp ^= item
    }
    
    return temp
}
```