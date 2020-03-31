---
layout: post
title: "[RxSwift] Share(replay:)"
category: 
  - RxSwift
comments: true
published: true
---

`anObservable.share(replay:1)`  같은 코드를 보신적 있으실 겁니다. `Share` 연산자는 언제 써야하는 걸까요? 

이 연산자를 알아 보기에 앞서 `Observable`의 특징 하나를 살펴보겠습니다.

## Observable의 특징
`Observable`은 매우 게으른 pull-driven 시퀀스 입니다. 무슨 뜻이냐구요? `Observable`에 아무리 여러 연산자를 호출해도 `subscribe()` 가 호출되기 전까진 아무 동작도 하지 않습니다. 진짜냐구요? 

예제를 보시죠.

**[코드]**

```swift
let numObservable = Observable.of(1,2,3,4,5).debug()    
numObservable
    .map { $0 + 1 }
```

**[실행결과]**

```
// 아무것도 출력되지 않음
```

`Observable`에 `debug()`를 걸어 두었지만 시퀀스에서 아무런 아이템도 방출되지 않는걸 확인할 수 있습니다.

![Share]({{ site.baseurl }}/images/2020/Rxswift - Share1.png)

이제 `Observable`의 연산자 뒤에 `subscribe()`를 호출해보겠습니다. 

**[코드]**

```swift
let disposeBag = DisposeBag()    
let numObservable = Observable.of(1,2,3,4,5).debug("")    
numObservable
    .map { $0 + 1 }
    .subscribe()  // subscribe()를 호출
    .disposed(by: disposeBag)
```

**[실행결과]**

```
-> subscribed
-> Event next(1)
-> Event next(2)
-> Event next(3)
-> Event next(4)
-> Event next(5)
-> Event completed
-> isDisposed
```
이제서야 `Observable`의 시퀀스에서 아이템이 방출되는 것을 확인할 수 있습니다.

![Share]({{ site.baseurl }}/images/2020/Rxswift - Share2.png)

`Observable`은 `subscribe` 될때마다 `create` 클로저를 호출해 `Observable`을 생성합니다. (참고 : [Subscribe](https://jusung.github.io/Subscribe//)) 다시말하면 **`Observable`에 `Subscribe`를 한 횟수만큼 `Observable`이 생성**됩니다.

> `Observable`을 `Subscribe`할때마다 새로운 `Observable`시퀀스가 생성된다.  

자 이제 이 사실을 기억하고  `share()`연산자를 살펴보겠습니다.

## Share()

다음과 같은 코드가 있습니다.

**[코드]**

```swift
class ViewController: UIViewController {
    @IBOutlet weak var requestMoreButton: UIButton!
    @IBOutlet weak var remainCountLabel: UILabel!
    
    let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        bind()
    }
    
    private func bind() {
        // 서버에 API를 요청해 결과를 반환하는 시퀀스라 가정
        let networkRequestAPI = Observable.of(100).debug("networkRequestAPI")

        let result = requestMoreButton.rx.tap
            .flatMap { networkRequestAPI }
        
        result
            .map { $0 > 0 }
            .bind(to: requestMoreButton.rx.isHidden)
            .disposed(by: disposeBag)

        // bind(to:)는subscribe()의 별칭(Alias)으로 Subscribe()를 호출한 것과 동일
        result
            .map { "Count:\($0)" }
            .bind(to: remainCountLabel.rx.text)
            .disposed(by: disposeBag)
    }
}
```
버튼을 누르면 서버에 API 요청을 보내 결과를 받고,  그 결과를 사용해 버튼을 보여주거나 감추고 문구를 표시합니다. 혹시 위 코드에서 문제점을 발견하셨나요?

코드를 실행해서 버튼을 누르면 다음과 같은 결과를 확인할 수 있습니다.

**[실행결과]**

```
networkRequestAPI -> subscribed
networkRequestAPI -> Event next(100)
networkRequestAPI -> Event completed
networkRequestAPI -> isDisposed
networkRequestAPI -> subscribed
networkRequestAPI -> Event next(100)
networkRequestAPI -> Event completed
networkRequestAPI -> isDisposed
```

![Share]({{ site.baseurl }}/images/2020/Rxswift - Share3.png)

보시다시피 서버에 **API를 두번 요청**해 결과를 받아왔습니다. 만약 이 API 요청의 결과를 `Subscribe()`해 사용하는 곳의 갯수가 10개가 더 늘어난다면 어떻게 될까요? 서버에 API를 12번 요청하게 됩니다. 😰

이건 저희가 기대하는 동작은 아닐 것입니다. 이 문제를 해결하기 위해 `share()` 연산자를 사용합니다. 

> `share()` 연산자를 사용하면 `Subscribe()`할때마다 새로운 `Observable` 시퀀스가 생성되지 않고, **하나의 시퀀스에서 방출되는 아이템을 공유해 사용**할 수 있습니다.

위 예제 코드에 다음과 같이 `.share()`를 추가하고 코드를 다시 실행해보도록 하겠습니다.

**[코드]**

```swift
let result = requestMoreButton.rx.tap
	.flatMap { networkRequestAPI }
	.share() // result 시퀀스를 공유하도록 처리
```

**[실행결과]**

```
networkRequestAPI -> subscribed
networkRequestAPI -> Event next(100)
networkRequestAPI -> Event completed
networkRequestAPI -> isDisposed
```

저희가 기대한대로 단 한번의 API 호출이 이루어졌음을 확인할 수 있습니다.

![Share]({{ site.baseurl }}/images/2020/Rxswift - Share4.png)

`share()`는 `subscribe()`가 처음 호출될 때 (Subscriptoin횟수가 0 -> 1 일때)만 Subscription을 생성하고, 이후 두번째 세번째로 `subscribe()`가 호출되면 새로운 Subscription을 생성하는 것 대신 이미 만들어진 Subscription을 이후 `subscribe()`를 호출한 곳에 공유해 사용합니다. 

만약 `share()`에 `subscribe()`한 Subscription이 모두 `disposed` 되면 `share()`는 공유했던 Subscription을 `dispose` 시킵니다.

이후 다른  `subscribe()`가 호출되면 `share()`는 새로운 Subscription을 생성합니다.
그렇기 때문에 `share()`는 completed 되지 않는 `Observable`에 사용하는 것이 안전합니다. 혹은 공유하는 시퀀스가 completed 된 후 새로운 `Observable`이 생성되지 않는다고 확신할 때 사용해야합니다.

## Reply

`share(replay: 1)`에서 `replay`에 넣는 인자는 버퍼의 크기를 의미합니다.

다른 시퀀스에서 `share()`된 `Observable`을 구독했을 때, 가장 **최근 방출했던 아이템을 버퍼의 크기만큼 새로운 구독 시퀀스에 전달**해줍니다. `share()`연산자의 선언을 살펴보면 다음과 같습니다.

**[RxSwift/SubjectLifetimeScope.swift]**

```swift
public func share(replay: Int = 0, scope: SubjectLifetimeScope = .whileConnected)
```

`replay`외에 `scope`이라는 인자가 있는데 이 값은 버퍼의 생명주기에 관한 것입니다. 입니다.
 `.forever`와 `.whileConnected`를 선택할 수 있고 아무값도 설정하지 않으면 .`whileConnected`를 기본값으로 사용합니다.

- `.forever` : Subscription이 0이 되더라도 버퍼가 유지 됩니다. 그래서 새로운 Subscription은 `Subscribe()` 를 하면 **마지막에 버퍼에 남아있던 replay개수 만큼의 값을 수신**하게 됩니다.

- `.whileConnected` : 1개 이상의 Subscriber가 존재하는 동안만 버퍼가 유지 됩니다. Subscription이 0이 되면 버퍼가 비워지고 새로운 Subscription은 버퍼에 남아 있던 값이 없으므로 `replay`시 **새 값을 요청해 수신**하게 됩니다.

## 한걸음 더 들어가기

사실 `share()`에 대해 여기까지만 알아도 사용하는데는 지장이 없습니다. 하지만 `share()`가 어떻게 동작하는지 좀 더 알고 싶으신 분은 위해 좀 더 깊이 들어가 보겠습니다.

앞서 살펴봤던 `share()`함수의 정의를 구현부까지 살펴보죠.

**[RxSwift/SubjectLifetimeScope.swift]**

```swift
public func share(replay: Int = 0, scope: SubjectLifetimeScope = .whileConnected)
        -> Observable<Element> {
        switch scope {
        case .forever:
            switch replay {
            case 0: return self.multicast(PublishSubject()).refCount()
            default: return self.multicast(ReplaySubject.create(bufferSize: replay)).refCount()
            }
        case .whileConnected:
            switch replay {
            case 0: return ShareWhileConnected(source: self.asObservable())
            case 1: return ShareReplay1WhileConnected(source: self.asObservable())
            default: return self.multicast(makeSubject: { ReplaySubject.create(bufferSize: replay) }).refCount()
            }
        }
    }
```

코드를 보면 `share(replay)`를 하면 scope가 `.forever` 이거나 `.whileConnected` 상관없이 모두  `return self.multicast(makeSubject: { ReplaySubject.create(bufferSize: replay) }).refCount()` 를 반환하는 것을 확인할 수 있습니다.

네 그렇습니다.
사실 `anObservable.share()`는 `anObservable.publish().refCount()`입니다.

다음 그림과 이후 설명을 보면 앞에 설명한 `share()`의 동작이 좀 더 이해 되실 겁니다. 더 자세한 설명은 생략합니다. 🙂

![Share]({{ site.baseurl }}/images/2020/Rxswift - Share5.png)

- `publish()`  : 이 연산자는 보통의 `Observable`을 `ConnectableObservable`로 변환해 줍니다.
- `ConnectableObservable` : `ConnectableObservable`은 Subscriber가 있어도 `connect()`를 호출하기 전까지는 아이템을 방출하지 않습니다. `connect()`를 호출하고 나서야 아이템을 방출하기 시작합니다.
- `refcount()`  : `refcount()` 는 `ConnectableObservable`에 Connect와 Disconnect를 자동으로 담당하고, `ConnectableObservable`을 보통의 `Observable`처럼 사용할 수 있게 해줍니다. 다시말해 Subscription count를 계속 세고 있다가 Subscription의 개수가 0 -> 1 개가 되는 시점에 `connect()`를 수행하고 Subscription이 0이 되면 `disconnect()`를 수행합니다.

## 결론

> 여러 시퀀스에서 사용하게 되는 `Observable`은 `Subscribe()` 할때마다 subscription이 생성되니 `share()`해서 사용해야 한다. 보통 `share(replay: 1)` 형태로 사용한다.

라는게 이번 포스트의 결론입니다.

이상 `Share(replay:)`에 대해 대해 알아 보았습니다. (👨🏻‍💻지식이 +5 늘었다.)

다음 포스트에서 또 만나요~ 🚀😄

**[참고]**

- [RxSwift - share(), share(replay:, scope:)에 대해서 알아보자 :: 고무망치의 Dev N Life](https://rhammer.tistory.com/307)
- [RxSwift: share()-ing is Caring - Gett Engineering - Medium](https://medium.com/gett-engineering/rxswift-share-ing-is-caring-341557714a2d)
- [RxSwift: share vs replay vs shareReplay](https://medium.com/@_achou/rxswift-share-vs-replay-vs-sharereplay-bea99ac42168)
- [RxJava Tip for the Day - Share, Publish, Refcount and All That Jazz](https://blog.kaush.co/2015/01/21/rxjava-tip-for-the-day-share-publish-refcount-and-all-that-jazz/)
- [Connectable Observable Operators](https://github.com/ReactiveX/RxJava/wiki/Connectable-Observable-Operators#connectableobservablerefcount)
- [ReactiveX - RefCount operator](http://reactivex.io/documentation/operators/refcount.html)
- [RxSwift, subscriptions 의 공유](https://brunch.co.kr/@tilltue/15)