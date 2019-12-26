---
layout: post
title: "[RxSwift] withLatestFrom"
category: 
  - RxSwift
tags: 
  - Combination Operators
comments: true
published: true
---

## 역할
두 `Observable`중 첫번째 `Observable`에서 아이템이 방출될 때마다 그 아이템을 두번째 `Observable`의 **가장 최근 아이템과 결합**해 방출합니다.

![withLatestFrom]({{ site.baseurl }}/images/2019/RxSwift - withLatestFrom.png)

## 예제

**[코드]**

```swift
let disposeBag = DisposeBag()
    
let numSubject  = PublishSubject<Int>()
let charSubject = PublishSubject<String>()
    
numSubject
    .withLatestFrom(charSubject) { "\($0)\($1)"}
    .subscribe(onNext: { print($0) })
    .disposed(by:disposeBag)
    
numSubject.onNext(1)     // 아이템 방출되지 않음 : charSubject에서 방출된 아이템이 아직 1개도 없으므로
    
charSubject.onNext("A")
    
numSubject.onNext(2)     // 2A
    
charSubject.onNext("B")
    
charSubject.onNext("C")
charSubject.onNext("D")
    
numSubject.onNext(3)     // 3D
numSubject.onNext(4)     // 4D
numSubject.onNext(5)     // 5D
```

**[실행 결과]**

```
2A
3D
4D
5D
```

`numSubject`에서 아이템이 방출될때만 `withLatestFrom` 연산이 수행됩니다. 첫번째 `Observable`에서 첫 아이템(A)이 방출된 시점에 두번째 `Observable`에서 방출된 아이템이 없으므로, 아무런 아이템이 방출되지 않고 그 이후 부터 아이템이 방출되는 것을 확인할 수 있습니다.

## 활용

첫번째 `Observable`: tap 이벤트

두번째 `Observable`: 이벤트에 대한 처리

두 `Observable`을 이렇게 구성 한 후 `withLatestFrom` 연산자를 이용해 결합하면, tap 이벤트가 발생 했을 때 필요한 데이터를 발생시키거나 관련된 처리를 하도록 사용할 수 있습니다.

![withLatestFromUserful]({{ site.baseurl }}/images/2019/RxSwift - withLatestFrom Useful.png)

**[코드]**

```swift
let disposeBag = DisposeBag()
    
let loginButton  = PublishSubject<Void>()
let usernameTextField = PublishSubject<String>()
    
loginButton
    .withLatestFrom(usernameTextField)
    .subscribe(onNext: { print($0) })
    .disposed(by:disposeBag)
    
usernameTextField.onNext("tome.kye")
    
loginButton.onNext(())      // "tome.kye" 아이템 방출
    
usernameTextField.onNext("tommy.kye")
    
loginButton.onNext(())      // "tomme.kye" 아이템 방출
```

**[실행 결과]**

```
tome.kye
tommy.kye
```

이 예제는 로그인 버튼 선택시 데이터를 전달합니다. `.withLatestFrom(usernameTextField)` 는 첫번째 `Observable`의 아이템과 합성하지 않고, 두번째 `Observable` 아이템만 그대로 전달합니다.

아래 이 코드는

```swift
loginButton
    .withLatestFrom(usernameTextField)
    .subscribe(onNext: { print($0) })
    .disposed(by:disposeBag)
```

다음과 같이 API를 찌르는데 필요한 인자를 전달하는 용도로도 사용 가능합니다.

```swift
loginButton
    .withLatestFrom(usernameTextField)
    .flatMap { username -> Observable<UserDataModel> in
    		return API.Request(username: username)
    }
```


**[참 고]**

- [ReactiveX - withLatestFrom](https://rxmarbles.com/#withLatestFrom)
