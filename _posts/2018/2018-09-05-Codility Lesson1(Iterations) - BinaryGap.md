---
layout: post
title: "[Codility] Lesson1 (Iterations) - BinaryGap"
tags: 
  - Codility
comments: true
published: true
---

## 문제
A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N. 

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps. The number 32 has binary representation 100000 and has no binary gaps.

Write a function:

`
public func solution(_ N : Int) -> Int
`

that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5. Given N = 32 the function should return 0, because N has binary representation '100000' and thus no binary gaps.

Write an efficient algorithm for the following assumptions:

- N is an integer within the range [1..2,147,483,647].


## 문제 이해
BinaryGap은 이진수로 표현된 정수에서 1로 둘러싸인 가장 긴 연속된 0의 숫자를 찾는 문제입니다. 정수 9는 이진수로 `1001`이므로 9의 BinaryGap은 2입니다. 숫자 529는 이진수로 `1000010001`게 표현되는데 여기서는 2개의 BinaryGap이 존재합니다. BinaryGap은 가장 긴 연속된 0이므로 529의 BinaryGap는 4가 됩니다. `100000`나 `1111`는 1로 완전히 둘러싸여 있지 않거나 0이 없으므로 BinaryGap은 0이 됩니다.

## 해결 전략
Input이 1보다 큰 양수이고 Output은 이 숫자에 대한 BinaryGap을 반환 해야합니다.

1. 처음 1부터 다음 1이 나타날때까지 index의 차이를 계산해, 처음과 그 다음 1사이의 0의 개수를 계산합니다. 
2. 만약 이 0의 개수가 이전에 찾은 연속된 0의 개수보다 크면 그 변수를 더 큰 연속된 숫자로 변경합니다.
3. 변경하고 나서 1의 처음 index(startIndex)를 현재 찾은 index로 변경하고 그 다음 1이 나타나는 index를 찾아 앞의 1.과 2.의 단계를 반복합니다.

## 코드 (in Swift)

```swift
public func solution(_ N : Int) -> Int {
        let binaryString = String(N, radix: 2)  // 10진수를 2진수로 변환
     
        var startIndex = -1
        var currentBinaryGap = 0
        var binaryGap = 0
     
        for i in 0..<binaryString.characters.count { // 이진수를 1개씩 순회
            if binaryString[binaryString.index(binaryString.startIndex, offsetBy: i)] == "1" {
                if startIndex == -1 { // 입력에서 1이 처음으로 나타난 경우 그 index를 startIndex에 저장 
                    startIndex = i
                }
                else   {// 입력에서 1이 2번 이상 나타나는 경우
                    currentBinaryGap = i - startIndex - 1  // 이전 1이 나타난 index에서 현재 1이 나타난 index의 차이를 계산. 이진수는 1과 0으로만 구성돼 있기 때문에 이 차이가 곧 BinaryGap
     
                    if currentBinaryGap > binaryGap { // 최종 BinaryGap보다 현재 BinaryGap이 큰 경우 교체
                        binaryGap = currentBinaryGap
                    }
     
                    startIndex = i  // 현재 1이 나타난 index를 startIndex로 지정. 이후에 나타나는 1의 위치에 시작점으로 사용됨
                }
            }
        }
     
        return binaryGap
    }
```