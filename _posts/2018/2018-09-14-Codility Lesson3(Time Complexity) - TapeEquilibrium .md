---
layout: post
title: "[Codility] Lesson3 (Time Complexity) - TapeEquilibrium"
category: 
  - Codility
comments: true
published: true
---

## 문제
A non-empty array A consisting of N integers is given. Array A represents numbers on a tape.

Any integer P, such that 0 < P < N, splits this tape into two non-empty parts: `A[0], A[1], ..., A[P − 1] and A[P], A[P + 1], ..., A[N − 1]`.

The difference between the two parts is the value of: `|(A[0] + A[1] + ... + A[P − 1]) − (A[P] + A[P + 1] + ... + A[N − 1])|`

In other words, it is the absolute difference between the sum of the first part and the sum of the second part.

For example, consider array A such that:

```
  A[0] = 3
  A[1] = 1
  A[2] = 2
  A[3] = 4
  A[4] = 3
```
  
We can split this tape in four places:

- `P = 1, difference = |3 − 10| = 7`
- `P = 2, difference = |4 − 9| = 5` 
- `P = 3, difference = |6 − 7| = 1` 
- `P = 4, difference = |10 − 3| = 7`

Write a function:

`
public func solution(_ A : inout [Int]) -> Int
`

that, given a non-empty array A of N integers, returns the minimal difference that can be achieved.

For example, given:

```
  A[0] = 3
  A[1] = 1
  A[2] = 2
  A[3] = 4
  A[4] = 3
```
  
the function should return 1, as explained above.

Write an **efficient** algorithm for the following assumptions:

- N is an integer within the range [2..100,000];
- each element of array A is an integer within the range [−1,000..1,000].

## 문제 이해
P가 `|(A[0] + ... + A[P-1]) - (A[P] + ... + A[N-1])|` 로 정의돼 있습니다. N이 주어졌을 때, 배열에서 A[N]을 포함하는 왼쪽의 원소를 합한 값에서, A[N]의 오른쪽에 위치하는 원소의 값을 뺀 수의 절대값입니다. 문제는 가능한 모든 P(N)에 대해 가장 작은 값을 찾는 것입니다.


## 해결 전략
위 문제 설명의 예제를 보시면 한가지 패턴을 확인할 수 있습니다.

- `P = 1, difference = |3 − 10| = 7`
- `P = 2, difference = |4 − 9| = 5`
- `P = 3, difference = |6 − 7| = 1` 
- `P = 4, difference = |10 − 3| = 7` 

절대값 안의 숫자의 합이 13으로 같다는 것입니다. `3 - 10`에서 수식은 무시하고 3과 10을 더하면 13, `4 - 9`에서도 마찬가지로 4와 9를 더하면 13으로 갖습니다. 나머지도 마찬가지입니다. 여기서 우리는 이 패턴을 가지고 P(N)값을 구할 수 있습니다. 


## 코드 (in Swift)
```swift
public func solution(_ A : inout [Int]) -> Int {
    var totalSum = 0
    var minDifference = 0
    var P = 0
    
    for element in A {	// 배열 A에서 각 원소의 합을 더합니다.
        totalSum += element
    }
    
    for (index, _) in A.enumerated() {
        if index > 0 {
            P += A[index - 1] // P에 값에 A원소들을 하나씩 더합니다.
            
            let difference = abs((P * 2) - totalSum)  // P(N)을 구합니다. P값을 2배 한것에서 전체 합을 빼면 그 차이를 구할 수 있습니다.
            
            if index == 1 { // 첫번째 차이를 최소차이 변수에 저장합니다.
                minDifference = difference
            }
            
            if difference < minDifference { // 배열을 순회하면서 계산한 차이가 이전의 최소 차이보다 작은 경우 그 값을 최소 차이 변수에 할당합니다.
                minDifference = difference
            }
        }
    }
    
    return minDifference
}
```