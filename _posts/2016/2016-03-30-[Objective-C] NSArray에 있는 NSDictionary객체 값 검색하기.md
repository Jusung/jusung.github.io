---
layout: post
title: "[Objective-C] NSArray에 있는 NSDictionary객체 값 검색하기"
tags: 
  - Objective-C
comments: true
published: true
---

`NSDictionary`를 배열의 원소로 가지고 있는 `NSArray`가 있다면 배열의 원소가 특정 값을 가지고 있는지 어떻게 검색할 수 있을까요? `NSPredicate`를 이용하면 가능합니다.

코드는 다음과 같습니다.

`
[NSPredicate predicateWithFormat:@"%K == %@", key, value];
`

`NSDictionary`에 `key` 해당하는 해당하는 부분을 %K에 적어 주고 검색하고 싶은 해당 키의 값을 `%@`에 넣어 줍니다.

> @”%K == %@”
> 
> This predicate checks whether the value of the key %K is the same as the value of the object %@. The variables are supplied at runtime as arguments to predicateWithFormat:.
> 
> 참고 : [Format String Summury in Predicate Programming guide in Apple Docs](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Predicates/Articles/pCreating.html){:target="_blank"}

응용으로 `NSArray`내에 특정 값을 갖고 있는 `NSDictionary`객체가 존재하는지 유무를 검색하기 위해서 아래와 같이 `NSArray`의 카테고리를 만들어서 사용할 수 있습니다.

```objc
#import "NSArray+ContainsValue.h"

@implementation NSArray (ContainsValue)

- (BOOL)containsValueWithKey:(NSString *)key value:(id)value {
    NSArray *results = [self arrayContainsValueWithKey:key value:value];

    return [results count] > 0 ? YES : NO;
}

- (NSArray *)arrayContainsValueWithKey:(NSString *)key value:(id)value {
    NSPredicate *predicate = [NSPredicate predicateWithFormat:@"(%K == %@)",key,value];
    NSArray *results = [self filteredArrayUsingPredicate:predicate];

    return results;
}

@end
```