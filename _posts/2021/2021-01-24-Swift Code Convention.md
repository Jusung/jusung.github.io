---
layout: post
title: "팀에서 사용 중인 Swift Style Guide (코드 컨벤션)"
category:
  - Swift
tags:
  - Basic
comments: true
published: true
---

코드 컨벤션이 있으면 협업시 일관성있는 코드를 작성할 수 있어서 코드의 일관성 유지에 좋습니다. 또 다른 사람이 작성한 코드를 읽을 때 코드의 문법보다 로직에 집중할 수 있는 장점이 있습니다.

다음은 저희팀에서 사용 중인 Swift Style Guide입니다. 코드 컨벤션 결정시 참조하시면 좋을 것 같습니다. 

이 코드 컨벤션 작성시 다음과 같은 다른 컨벤션을 참조했습니다.

# Swift Style Guide

## 목차
- [1. 코드 포매팅](#1-코드-포매팅)
	- [1.1 임포트](#11-임포트)
	- [1.2 빈 줄](#12-빈줄)
	- [1.3 들여쓰기](#13-들여쓰기)
	- [1.4 띄어쓰기](#14-띄어쓰기)
	- [1.5 기타](#15-기타)

- [2. 네이밍](#2-네이밍)
	- [2.1 일반](#21-일반)
	- [2.2 클래스](#22-클래스)
	- [2.3 함수](#23-함수)
	- [2.4 약어](#24-약어)

- [3. 코드 스타일](#3-기타)
	- [3.1 클로저](#31-클로저)
	- [3.2 클래스와 구조체](#32-클래스와-구조체)
	- [3.3 타입](#33-타입)
	- [3.4 타입추론 사용](#34-타입추론-사용)
	- [3.5 self](#35-self)
	- [3.6 튜플](#36-튜플)
	- [3.7 패턴](#37-패턴)
 	- [3.8 제네릭](#38-제네릭)
	- [3.9 static](#39-static)
	- [3.10 final](#310-final)
	- [3.11 프로토콜 extension](#311-프로토콜-extension)
	- [3.12 switch-case](#312-switch-case)
	- [3.13 return](#313-return)
	- [3.14 사용하지 않는 코드](#314-사용하지-않는-코드)

## [1. 코드 포매팅]

### 1.1 임포트

- 모듈 임포트는 알파벳 순으로 정렬합니다. 내장 프레임워크를 먼저 임포트하고, 빈 줄로 구분해 3rd-party프레임워크를 임포트 합니다.

```swift
import UIKit

import SwiftyColor
import SwiftyImage
import Then
import URLNavigator
```

- 파일이 필요로하는 최소의 모듈만 임포트 합니다. 예를들어, `Foundation`으로 충분하면 `UIKit`은 임포트 하지 않습니다.

✅ Preferred 
	
```swift
import UIKit

var view: UIView
var deviceModels: [String]
```

✅ Preferred 

```swift
import Foundation

var deviceModels: [String]
```

⛔️ Not Preferred 

```swift
import UIKit
import Foundation

var view: UIView
var deviceModels: [String]
```

⛔️ Not Preferred

```swift
import UIKit

var deviceModels: [String]
```

- 모듈의 상세까지 지정할 수 있으면 지정합니다.

✅ Preferred 

```swift
import struct SwiftyJSON.JSON
import struct CoreLocation.CLLocation.CLLocationCoordinate2D
```

⛔️ Not Preferred 

```swift
import SwiftyJSON
import CoreLocation
```

### 1.2 빈줄
- 빈 줄에는 공백이 포함되지 않도록 합니다.
- 모든 파일은 빈 줄로 끝나도록 합니다.

### 1.3 들여쓰기
- 탭을 눌렀을시 4개의 space를 사용합니다.
- 들여쓰기는 Xcode에서 제공하는 `^ + i` 를 눌렀을 때, 적용되는 space를 사용합니다.
- 최대 가로 길이는 100 characters를 사용합니다.

### 1.4 띄어쓰기
- 콜론(`:`)을 사용할때는 콜론의 오른쪽에만 공백을 둡니다.
	

[상수]

✅ Preferred

```swift
let names: [String: String]?
```

⛔️ Not Preferred

```swift
let names: [String:String]?
let names: [String : String]?
```

[클래스]

✅ Preferred 

```swift
class MyClass: SuperClass {
  // ...
}
```

⛔️ Not Preferred

```swift
class MyClass : SuperClass {
  // ...
}
```

- 삼항연산자의 경우 콜론 앞뒤로 띄웁니다.

✅ Preferred 

```swift
func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
	return shouldRotate ? .allButUpsideDown : .portrait
}
```

⛔️ Not Preferred 

```swift
func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
	return shouldRotate ? .allButUpsideDown: .portrait
}
```

[기타]

✅ Preferred 

```swift
// specifying type
let pirateViewController: PirateViewController

// dictionary syntax (note that we left-align as opposed to aligning colons)
let ninjaDictionary: [String: AnyObject] = [
    "fightLikeDairyFarmer": false,
    "disgusting": true
]

// declaring a function
func myFunction<T, U: SomeProtocol>(firstArgument: U, secondArgument: T) where T.RelatedType == U {
    /* ... */
}

// calling a function
someFunction(someArgument: "Kitten")

// superclasses
class PirateViewController: UIViewController {
    /* ... */
}

// protocols
extension PirateViewController: UITableViewDataSource {
    /* ... */
}
```

- `if let`, `guard let` 구문이 긴 경우에는 줄바꿈하고 한 칸 들여씁니다.

✅ Preferred 

```swift
if let user = self.veryLongFunctionNameWhichReturnOptionalUser(),
 let name = user.veryLongFunctionNameWhichReturnsOptionlName(),
 user.gender == .female {
//  ...
 }

guard let user = self.veryLongFunctionNameWhichReturnsOptionalUser(),
  let name = user.veryLongFunctionNameWhichReturnsOptionalName(),
  user.gender == .female else {
  return
}
```

- 일반적으로 콤마(`,`) 뒤에는 공백을 추가합니다.

✅ Preferred 

```swift
let myArray = [1, 2, 3, 4, 5]
```

⛔️ Not Preferred

```swift
let myArray = [1,2,3,4,5]
```

- 연산자 앞뒤로 공백을 추가합니다.


✅ Preferred 

```swift
let myValue = 20 + (30 / 2) * 3
```

⛔️ Not Preferred

```swift
let myValue = 20+(30/2)*3
```

- 화살표 양쪽에 가독성을 위해 빈 공백을 추가합니다.

[함수 리턴 타입]

✅ Preferred

```swift
func doSomething() -> String {
  // ...
}
```

⛔️ Not Preferred

```swift
func doSomething()->String {
  // ...
}
```

[클로저 리턴 타입]

✅ Preferred 

```swift
func doSomething(completion: () -> Void) {
  // ...
}
```

⛔️ Not Preferred

```swift
func doSomething(completion: ()->Void) {
  // ...
}
```

[Rx]

✅ Preferred 

```swift
.map { $0.call.farePaymentType }
.map { method -> String? in
    guard let name = method.name else { return nil }
    return Local.Taxi.taxiDispatchingPaidItemApply.string.phrase(["item_name": name])
}
```

⛔️ Not Preferred 

```swift
.map {$0.call.farePaymentType}
.map {method -> String? in
    guard let name = method.name else { return nil }
    return Local.Taxi.taxiDispatchingPaidItemApply.string.phrase(["item_name": name])
}
```

✅ Preferred 

```swift
var bizGroupHidden: Driver<Bool> {
    return self.callInfo.asObservable()
        .map { $0.call.bizGroup }
        .map { $0 == nil }
        .asDriverJustComplete()
}
```

⛔️ Not Preferred 

```swift
var bizGroupHidden:Driver<Bool> {
    return self.callInfo.asObservable()
        .map { $0.call.bizGroup }
        .map { $0 == nil }
        .asDriverJustComplete()
}
```

### 1.5 기타

- 불필요한 괄호는 생략합니다.

✅ Preferred 

```swift
if userCount > 0 { ... }
switch someValue { ... }
let evens = userCounts.filter { number in number % 2 == 0 }
let squares = userCounts.map { $0 * $0 }
```

⛔️ Not Preferred 

```swift
if (userCount > 0) { ... }
switch (someValue) { ... }
let evens = userCounts.filter { (number) in number % 2 == 0 }
let squares = userCounts.map() { $0 * $0 }
```

- `enum`의 연관값을 사용하지 않는 경우는 생략합니다.

✅ Preferred 

```swift
if case .done = result { ... }

switch animal {
case .dog:
  ...
}
```

⛔️ Not Preferred 

```swift
if case .done(_) = result { ... }

switch animal {
case .dog(_, _, _):
  ...
}
```

## 2. 네이밍

- 묘사를 잘하고 일관된 네이밍은 코드를 읽고 이해하기 쉽게 해줍니다. 네이밍은 Apple의 [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)을 따릅니다.
- 클래스(타입, 프로토콜 이름 포함) 이름에는 UpperCamelCase(첫 문자를 대문자로 시작하는 camel표기법), 함수 이름에는 camelCase를 사용합니다.


### 2.1 일반

[일반]

✅ Preferred 

```swift
protocol SpaceThing {
  // ...
}

class SpaceFleet: SpaceThing {

  enum Formation {
    // ...
  }

  class Spaceship {
    // ...
  }

  var ships: [Spaceship] = []
  static let worldName: String = "Earth"

  func addShip(_ ship: Spaceship) {
    // ...
  }
}

let myFleet = SpaceFleet()
```

[변수/상수]

- 일반변수 / 상수인 경우 따로 접두사를 붙이지 않습니다.

✅ Preferred 

```swift
let maximumNumberOfLines = 3
```

⛔️ Not Preferred 	

```swift
let kMaximumNumberOfLines = 3
let MAX_LINES = 3
```

- `static` 상수인 경우 앞에 k를 붙여줍니다.

✅ Preferred 

```swift
static let kMaximumNumberOfLines = 3
```

⛔️ Not Preferred 

```swift
static let maximumNumberOfLines = 3
```

[열거형]

✅ Preferred 

```swift
enum Result {
  case success
  case failure
}
```

⛔️ Not Preferred 	

```swift
enum Result {
  case Success
  case Failure
}
```

[RxSwift]

- RxSwift의 Subject, Driver, ControlerProperty, ControlEvent 등은 따로 접미사를 붙이지 않습니다.

✅ Preferred 

```swift
let recommendItem = BehaviorRelay<RecommendRideItem?>(value: nil)
let optionSwitch = PublishSubject<Void>()
let showRideSuggestion = PublishSubject<Void>()
```

⛔️ Not Preferred 

```swift
let recommendItem = BehaviorRelay<RecommendRideItem?>(value: nil)
let optionSwitchSignal = PublishSubject<Void>()
let showRideSuggestionPS = PublishSubject<Void>()
```


- 일반적인 부분이 앞에두고 구체적인 부분을 뒤에 둡니다.

✅ Preferred 

```swift
let titleMarginRight: CGFloat
let titleMarginLeft: CGFloat
let bodyMarginRight: CGFloat
let bodyMarginLeft: CGFloat
```

⛔️ Not Preferred

```swift
let rightTitleMargin: CGFloat
let leftTitleMargin: CGFloat
let bodyRightMargin: CGFloat
let bodyLeftMargin: CGFloat
```

- 생략시 사용이 모호해지는 타입은 이름에 타입에 대한 힌트를 포함시킵니다.

✅ Preferred 

```swift
let titleText: String
let cancelButton: UIButton
```

⛔️ Not Preferred 

```swift
let title: String
let cancel: UIButton
```

### 2.2 클래스

- 함수 이름에는 되도록 `get`을 붙이지 않습니다.

✅ Preferred

```swift
func name(for user: User) -> String?
```

⛔️ Not Preferred

```swift
func getName(for user: User) -> String?
```

### 2.3 함수

- 액션 함수의 네이밍은 '주어 + 동사 + 목적어' 형태를 사용합니다.
	- will은 특정 행위가 일어나기 직전이고, did는 특정 행위가 일어난 직후입니다.

✅ Preferred 

```swift
func backButtonDidTap() {
  // ...
}
```

⛔️ Not Preferred 	

```swift
func back() {
  // ...
}

func pressBack() {
  // ...
}
```

### 2.4 약어

- 약어로 시작하는 경우 소문자로 표기하고, 그 외 경우에는 항상 대문자로 표기합니다.

[예1]

✅ Preferred 

```swift
  let userID: Int?
  let html: String?
  let websiteURL: URL?
  let urlString: String?
```

⛔️ Not Preferred 	

```swift
let userId: Int?
 let HTML: String?
 let websiteUrl: NSURL?
 let URLString: String?
```

[예2]

✅ Preferred 

```swift
class URLValidator {

  func isValidURL(_ url: URL) -> Bool {
    // ...
  }

  func isProfileURL(_ url: URL, for userID: String) -> Bool {
    // ...
  }
}

let urlValidator = URLValidator()
let isProfile = urlValidator.isProfileUrl(urlToTest, userID: idOfUser)
```

⛔️ Not Preferred 

```swift
class UrlValidator {

  func isValidUrl(_ URL: URL) -> Bool {
    // ...
  }

  func isProfileUrl(_ URL: URL, for userId: String) -> Bool {
    // ...
  }
}

let URLValidator = UrlValidator()
let isProfile = URLValidator.isProfileUrl(URLToTest, userId: IDOfUser)
```
## 3. 기타
### 3.1 클로저

- 파라미터와 리턴 타입이 없는 클로저 정의시에는 `() -> Void` 를 사용합니다.

✅ Preferred

```swift
let completionBlock: (() -> Void)?
```

⛔️ Not Preferred 

```swift
let completionBlock: (() -> ())?
let completionBlock: ((Void) -> (Void))?
```

- 클로저 정의시 파라미터에는 괄호를 사용하지 않습니다.

✅ Preferred

```swift
{ operation, responseObject in
  // doSomething()
}
```

⛔️ Not Preferred 

```swift
{ (operation, responseObject) in
  // doSomething()
}
```

- 클로저 정의시 가능한 경우 타입 정의를 생략합니다.

✅ Preferred 

```swift
completion: { finished in
  // doSomething()
}
```

⛔️ Not Preferred 

```swift
completion: { (finished: Bool) -> Void in
  // doSomething()
}
```

- 클로저 호출시 또 다른 유일한 클로저를 마지막 파라미터로 받는 경우, 파라미터 이름을 생략합니다.

✅ Preferred 

```swift
UIView.animate(withDuration: 0.5) {
  // doSomething()
}
```

⛔️ Not Preferred 

```swift
UIView.animate(withDuration: 0.5, animations: { () -> Void in
  // doSomething()
})
```

- 사용하지 않는 파라미터는 `_`를 사용해 표시합니다.

✅ Preferred 

```swift
someAsyncThing() { _, _, argument3 in
  print(argument3)
}
```

⛔️ Not Preferred 

```swift
// WRONG
someAsyncThing() { argument1, argument2, argument3 in
  print(argument3)
}
```

- 한줄 클로저는 반드시 각 괄호 양쪽을 공백을 추가해야 합니다.

✅ Preferred 
```swift
let evenSquares = numbers.filter { $0 % 2 == 0 }.map { $0 * $0 }
```

⛔️ Not Preferred 
```swift
let evenSquares = numbers.filter {$0 % 2 == 0}.map {  $0 * $0  }
```

### 3.2 클래스와 구조체

- 구조체를 생성할 때는 Swift 구조체 생성자를 사용합니다.

✅ Preferred 

```swift
let frame = CGRect(x: 0, y: 0, width: 100, height: 100)
```

⛔️ Not Preferred 

```swift
let frame = CGRectMake(0, 0, 100, 100)
```

### 3.3 타입

- `Array<T>`와, `Dictionary<T: U>` 보다는 `[T]`, `[T: U]`를 사용합니다.

✅ Preferred 

```swift
var messages: [String]?
var names: [Int: String]?
```

⛔️ Not Preferred 

```swift
var messages: Array<String>?
var names: Dictionary<Int, String>?
```

### 3.4 타입추론 사용

- 컴파일러가 문맥속에서 타입을 추론할 수 있으면 더 간결한 코드를 위해 타입을 생략합니다.

✅ Preferred 

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

⛔️ Not Preferred

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### 3.5 self

- 문법의 모호함을 제거하기 위해 언어에서 필수로 요구하지 않는 이상 `self`는 사용하지 않습니다.

```swift
final class Listing {
  private let isFamilyFriendly: Bool
  private var capacity: Int
  
  init(capacity: Int, allowsPets: Bool) {
    ✅ Preferred 
    self.capacity = capacity
    isFamilyFriendly = !allowsPets

    ⛔️ Not Preferred 
    self.capacity = capacity
    self.isFamilyFriendly = !allowsPets // `self.` not required here
  }

  private func increaseCapacity(by amount: Int) {
    ✅ Preferred 
    capacity += amount

    ⛔️ Not Preferred 
    self.capacity += amount

    ✅ Preferred 
    save()
    
    ⛔️ Not Preferred 
    self.save()
  }
}
```

✅ Preferred 

```swift
TaxiPush.progressing.asPushActionObservable(callInfo.value.call.id)
            .map { $0.progressing }.unwrap()
            .map { $0/60 }
            .bind(to: timeRadius)
            .disposed(by: disposeBag)
```


⛔️ Not Preferred 

```swift
TaxiPush.progressing.asPushActionObservable(callInfo.value.call.id)
            .map { $0.progressing }.unwrap()
            .map { $0/60 }
            .bind(to: self.timeRadius)
            .disposed(by: self.disposeBag)
```



### 3.6 튜플

- 튜플의 맴버에는 명확성을 위해 이름을 붙여줍니다. 만약 필드가 3개를 넘는 경우 `struct`를 사용을 고려해보는 것을 권장합니다.

✅ Preferred 

```swift
func whatever() -> (x: Int, y: Int) {
  return (x: 4, y: 4)
}
```

⛔️ Not Preferred 

```swift
func whatever() -> (Int, Int) {
  return (4, 4)
}
let thing = whatever()
print(thing.0)
```

✅ Okay

```swift
func whatever2() -> (x: Int, y: Int) {
  let x = 4
  let y = 4
  return (x, y)
}

let coord = whatever()
coord.x
coord.y
```

✅ Preferred 

```swift
.map{($0.coord, nil, false)}
.withLatestFrom(viewModel.swapController) { (mapInfo: $0, swapController: $1) }
.filter { guard case .search = $0.swapController else { return false}; return true }
.map{$0.0},
```

⛔️ Not Preferred 

```swift
.map{($0.coord, nil, false)}
.withLatestFrom(viewModel.swapController) {($0, $1)}
.filter{guard case .search = $0.1 else {return false}; return true}
.map{$0.0},
```


### 3.7 패턴

- 프로퍼티의 초기화는 가능하면 `init`에서하고 가능하면 unwrapped Optionl의 사용을 지양합니다.

✅ Preferred 

```swift
class MyClass: NSObject {

  init() {
    someValue = 0
    super.init()
  }

  var someValue: Int
}
```

⛔️ Not Preferred 

```swift
class MyClass: NSObject {

  init() {
    super.init()
  }

  var someValue: Int?
}
```

### 3.8 제네릭

- 제네릭 타입 파라미터는 대문자를 사용하고 묘사적이어야 합니다. 타입 이름이 의미있는 관계나 역할을 갖지 않는 경우에만 `T`, `U` 혹은 `V` 같은 전형적인 단일 대문자를 사용하고 그 외에는 의미있는 이름을 사용합니다.

✅ Preferred 

```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

⛔️ Not Preferred 

```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### 3.9 static

- 디폴트 타입 매소드는 `static`을 사용합니다.

✅ Preferred 

```swift
class Fruit {
  static func eatFruits(_ fruits: [Fruit]) { ... }
}
```

⛔️ Not Preferred 

```swift
class Fruit {
  class func eatFruits(_ fruits: [Fruit]) { ... }
}
```


### 3.10 final

- 더 이상 상속이 발생하지 않는 클래스는 항상 `final` 키워드로 선언합니다.

✅ Preferred 
```
final class SettingsRepository {
  // ...
}
```

⛔️ Not Preferred 

```swift
class SettingsRepository {
  // ...
}
```

### 3.11 프로토콜 extension

- 프로토콜을 적용할 때는 extension을 만들어서 관련된 매소드를 모아둡니다.

✅ Preferred 

```swift
final class MyViewController: UIViewController {
  // ...
}

// MARK: - UITableViewDataSource

extension MyViewController: UITableViewDataSource {
  // ...
}

// MARK: - UITableViewDelegate

extension MyViewController: UITableViewDelegate {
  // ...
}
```

⛔️ Not Preferred

```swift
final class MyViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
  // ...
}
```

### 3.12 switch-case

- `switch-case`에서 가능한 경우 `default`를 사용하지 않습니다.
- 새로운 `case`가 생성됐을때 인지하지 못한 상태에서 `default`로 처리되지 않고 의도적으로 처리를 지정해 주기 위함입니다.

✅ Preferred 

```swift
switch anEnum {
case .a:
  // Do something
case .b, .c:
  // Do something else.
}
```

⛔️ Not Preferred 

```swift
switch anEnum {
case .a:
  // Do something
default:
  // Do something else.
}
```

### 3.13 return 

- `return`은 생략하지 않습니다.

✅ Preferred 

```swift
["1", "2", "3"].compactMap { return Int($0) }

var size: CGSize {
  return CGSize(
    width: 100.0,
    height: 100.0)
}

func makeInfoAlert(message: String) -> UIAlertController {
  return UIAlertController(
    title: "ℹ️ Info",
    message: message,
    preferredStyle: .alert)
}
```

⛔️ Not Preferred 

```swift
["1", "2", "3"].compactMap { Int($0) }

var size: CGSize {
  CGSize(
    width: 100.0,
    height: 100.0)
}

func makeInfoAlert(message: String) -> UIAlertController {
  UIAlertController(
    title: "ℹ️ Info",
    message: message,
    preferredStyle: .alert)
}
```

### 3.14 사용하지 않는 코드

- Xcode가 자동으로 생성한 템플릿을 포함한 사용하지 않는 코드는 placeholder 코멘트를 포함해 모두 제거합니다.

✅ Preferred 

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

⛔️ Not Preferred 

```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```
