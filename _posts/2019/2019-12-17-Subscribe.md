---
layout: post
title: "Subscribe의 동작 파헤치기"
category: 
  - RxSwift
tag:
  - RxSwift Operator
comments: true
published: true
---

## Subscribe의 일반적인 사용
RxSwift를 사용한다면 예외 없이 `Subscribe`를 사용하고 계실 것입니다.

보통 다음같이 사용하죠.

**[Subscribe의 사용]**

```swift
let numbersObservable = Observable.of(1, 2, 3, 4).debug()

numbersObservable
.subscribe()
.disposed(by: disposeBag)
```

**[코드 실행결과]**

```
-> subscribed**
-> Event next(1)**
-> Event next(2)**
-> Event next(3)**
-> Event next(4)**
-> Event completed**
-> isDisposed**
```

익숙하다구요? 당연하다구요? 

그렇다면 한가지 질문을 드리겠습니다.

`Observable`은 보이는데  `Observer`는 어딨을까요?

보통 `Subject`를 `Observable`이면서 `Observer`라고 얘기하는데, 여기서 말하는 `Observer`를 한번이라도 본적 있으신가요? 전 없는데…

 `Observer` 는 어딨는 걸까요? 🤔

## Subscribe의 정의 
공식 문서를 한번 살펴보겠습니다[^1]. 설명의 처음 한 문단만 보겠습니다.

> **Subscribe**  
**operate upon the emissions and notifications from an Observable**  
The Subscribe operator is the glue that connects an observer to an Observable. In order for an observer to see the items being emitted by an Observable, or to receive error or completed notifications from the Observable, it must first subscribe to that Observable with this operator.  

------
> Subscribe는 Observable의 알림과 방출에 따라 동작한다.  
Subscribe 연산자는 Observable에 **Observer를 연결하는 접착제**다. Observable에서 아이템이 방출되거나 에러 혹은 완료 알림을 받기 위해서는 반드시 처음에 **이 연산자를 Observable에 사용해 구독**해야만 가능하다.  

Wha~~t???

역시 공식문서느님은 말해주고 계셨네요. 문서를 읽으며 알 수 있는 사실은 두가지 입니다.

하나는 `Subscribe`가 `Observable`에 `Observer`를 연결해준다는 것!
`Subscribe`가 `Observer`같이 느껴졌는데 `Subscribe`가 `Observer`는 아니였습니다.

다른 하나는 `Subscribe`가 `Operator`였다는 사실입니다. 😮 헐… 

`Observable`을  합치거나 발행된 아이템을 변형시키거나 걸러내는 행위를 하는 것만 `Operator`인줄 알았는데 `Subscribe`도 연산자였던 것입니다. 하는 일은 정의과 같이 `Observable`에 `Observer`를 붙여주는 일이죠.

오호! 지식이 늘었네요! 😙 (지식이 +1 늘었다.)

`Subscribe`가 `Observable`에 `Observer`를 붙여준다는 사실은 확인했는데, 아직까지 `Observer`가 어디있는지는 발견하지 못했습니다.

이제 뭘해야할까요? `Subscribe` 안의 코드를 한번 살펴보죠.

## Subscribe의 내부
**[ObservableType+Extensions.swift]**

```swift
public func subscribe(onNext: ((Element) -> Void)? = nil, onError: ((Swift.Error) -> Void)? = nil, onCompleted: (() -> Void)? = nil, onDisposed: (() -> Void)? = nil)
        -> Disposable {
            let disposable: Disposable
            
            if let disposed = onDisposed {
                disposable = Disposables.create(with: disposed)
            }
            else {
                disposable = Disposables.create()
            }
            
            #if DEBUG
                let synchronizationTracker = SynchronizationTracker()
            #endif
            
            let callStack = Hooks.recordCallStackOnError ? Hooks.customCaptureSubscriptionCallstack() : []
            
            let observer = AnonymousObserver<Element> { event in
                
                #if DEBUG
                    synchronizationTracker.register(synchronizationErrorMessage: .default)
                    defer { synchronizationTracker.unregister() }
                #endif
                
                switch event {
                case .next(let value):
                    onNext?(value)
                case .error(let error):
                    if let onError = onError {
                        onError(error)
                    }
                    else {
                        Hooks.defaultErrorHandler(callStack, error)
                    }
                    disposable.dispose()
                case .completed:
                    onCompleted?()
                    disposable.dispose()
                }
            }
            return Disposables.create(
                self.asObservable().subscribe(observer),
                disposable
            )
    }
```

저희가 위 코드에서 살펴볼 부분은 두 부분입니다. 하나는 여기

```swift
let observer = AnonymousObserver<Element> { event in
                
                #if DEBUG
                    synchronizationTracker.register(synchronizationErrorMessage: .default)
                    defer { synchronizationTracker.unregister() }
                #endif
                
                switch event {
                case .next(let value):
                    onNext?(value)
                case .error(let error):
                    if let onError = onError {
                        onError(error)
                    }
                    else {
                        Hooks.defaultErrorHandler(callStack, error)
                    }
                    disposable.dispose()
                case .completed:
                    onCompleted?()
                    disposable.dispose()
                }
            }
```

보시다시피 `Subscribe`안에서 `Observer`를 생성하고 있네요! `Observer`는 `event`를 종류별로 방출하는 역할을 하고 있네요. 말그대로 이벤트를 관찰하고 거기에 맞는 행위를 하는 `Observer`역할을 하는 진짜 `Observer`입니다.

그다음에 살펴볼 코드는 다음 코드입니다.

```swift
return Disposables.create(
                self.asObservable().subscribe(observer),
                disposable
            )
```
`Subscribe`를 하면 반환되는 것이 `Disposable`인데 `Subscribe`안에서 `self.asObservable().subscribe(observer)` 요런 인자를 만들어서 `Disposable`을 생성하고 있네요. 

`self.asObservable()`을 좀 더 뜯어보면

```swift
extension ObservableType {
    
    /// Default implementation of converting `ObservableType` to `Observable`.
    public func asObservable() -> Observable<Element> {
        // temporary workaround
        //return Observable.create(subscribe: self.subscribe)
        return Observable.create { o in
            return self.subscribe(o)
        }
    }
}
```
 `Observable`을 생성하고 `subscribe`를 한 구독체를 반환하고 그 `Observable`에  `.subscribe(observer)` 로 `Observer`를 붙인 구독체를 최종적으로 반환하는 것을 확인할 수 있습니다.

내부에서 `subscribe`를 두번하는 것에 대한 얘기는 여기서는 생략하고(확실히 잘 몰라서 😓) `Subscribe`를 했을때 발생하는 일을 정리해 보겠습니다.

## Subscribe가 하는 일
`Observable`을 `Subscribe`하면 `Subscribe`내부에서

>
1.  `Observer`를 생성하고 생성한 그  `Observer`를
2. 내부에서 생성한  `Observable`에 붙이고
3. 붙인 그 구독체를 반환한다.

이렇게 정리할 수 있습니다.


`Subscribe`가 하는 일을 기억해두세요. 

사실 이번 포스트는 다음 포스트인 `Observable`의 `Share`와 `Share(replay)`를 설명하기 위한 사전 포스트거든요. 😊

다음 포스트에서 또 만나요~ 

[^1]: [ReactiveX - Subscribe operator](http://reactivex.io/documentation/operators/subscribe.html){:target="_blank"}