---
layout: post
title: "[RxSwift] Zip"
category: 
  - RxSwift
tags: 
  - Combination Operators
comments: true
published: true
---

## 역할
두 개 이상의 `Observable`에서 **방출된 순서가 같은 아이템을 묶어서 하나의 아이템으로 방출**합니다.

![zip]({{ site.baseurl }}/images/2019/RxSwift - zip.png)

## 예제

**[코드]**

```swift
let disposeBag = DisposeBag()
    
let numSubject = PublishSubject<Int>()
let charSubject = PublishSubject<String>()
    
let zippedObservable = Observable.zip(numSubject, charSubject).debug("zippedObservable")
    
zippedObservable
    .subscribe()
    .disposed(by: disposeBag)
    
numSubject.onNext(1)
charSubject.onNext("A")
numSubject.onNext(2)
charSubject.onNext("B")
charSubject.onNext("C")
charSubject.onNext("D")
numSubject.onNext(3)
numSubject.onNext(4)
numSubject.onNext(5)
```

**[실행 결과]**

```
(1, "A")
(2, "B")
(3, "C")
(4, "D")
```

`numSubject`의 아이템 5에 대해서는 `charSubject`에서 같은 순서에 방출된 아이템이 없으므로 `zip` 결과로 아무런 아이템도 방출되지 않습니다.

## 활용
같은 순서쌍으로 처리해야하는 작업에 사용 가능 합니다.

예를들면, `UITableView`에서 선택한 Cell의 `indexPath`와 `model`에 대한 처리를 각각하고 싶을때 `zip`을 사용하면 편리합니다.

첫번째 `Observable`: UITableView에서 선택된 아이템 `indexPath`

두번째 `Observable`: UITableView에서 선택된 아이템 `model`


**[코드]**

```swift
Observable.zip(tableView.rx.itemSelected, tableView.rx.modelSelected(ServiceType.self))
.subscribe(onNext: { [weak self] (indexPath, serviceType) in
    self?.tableView.deselectRow(at: indexPath, animated: false)
    self?.selectedServiceType.accept(serviceType)
})
.disposed(by: disposeBag)
```

`indexPath`는 선택된 Cell을 deselect하기 위해 사용하고, `model`은 현재 선택된 데이터를 관리하기 위해 사용할 수 있습니다.

**[참 고]**

- [ReactiveX - zip](http://reactivex.io/documentation/operators/zip.html)
