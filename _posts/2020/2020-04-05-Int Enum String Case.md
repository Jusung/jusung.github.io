---
layout: post
title: "[Swift] Int타입의 Enum에서 String case 얻기"
category: 
  - Swift
tag:
  - Standard Library
comments: true
published: true
---

만약 다음과 같은 `Int`타입의 `Enum`에서 각 case를 `String`으로 얻고 싶다면 어떻게 할 수 있을까요?

```swift
enum Vehicles: Int {
    case car
    case bus
    case taxi
}
```

일감으로 떠오르는 방법은 각 case 별로 `String`값을 반환하는 프로퍼티를 하나 만드는 것일 겁니다. 

```swift
enum Vehicles: Int {
    case car
    case bus
    case taxi
    
    var kind: String {
        switch self {
        case .car:
            return "car"
        case .bus:
            return "bus"
        case .taxi:
            return "taxi"
        }
    }
```

하지만 이 방법은 좋지 않은 방법입니다. 이유는 case가 변경될때마다 프로퍼티를 변경해줘야 하기 때문입니다.. 보다 간단한 방법은 없을까요? 있습니다. `String(describing: self)`를 사용하는 것입니다.

```swift
enum Vehicles: Int {
    case car
    case bus
    case taxi
    
    var kind: String {
        String(describing: self)
    }
}
```

`String(describing: Subject)`는 입력한 인스턴스에 대해 그것을 표현하는 `String` 을 반환합니다. 

Swift 표준라이브러리에서는 enum에 대한 `String(describing: Subject)`값으로 그 case의 이름을 값을 반환하도록 정의돼 있습니다.
각 case의 kind를 출력하면 기대했던 값이 나오는 것을 확인할 수 있습니다.

**[코드]**

```swift
enum Vehicles: Int, CaseIterable {
    case car
    case bus
    case taxi
    
    var kind: String {
        String(describing: self)
    }
}

Vehicles.allCases.forEach {
    print($0.kind)
}
```

**[실행결과]**

```
car
bus
taxi
```

이상 Int Enum 에서 case String 얻는 방법에 대해 알아보았습니다. (👨🏻‍💻지식이 +0.1 늘었다.)

다음 포스트에서 또 만나요~ 🚀😄