---
layout: post
title: "[Cocoa] NSMenuItem Title에 특정 폰트 적용하기"
tags: 
  - Cocoa
comments: true
published: true
---

이번 포스트에서는 `NSMenuItem` Title에 폰트를 적용하는 법을 알아보도록 하겠습니다.

기본적으로 `NSMenuItem` Title은 System font를 사용합니다.

![](https://farm2.staticflickr.com/1909/43233799340_9afe445339_o.png){: .center-image}
*기본 스타일*
{: style="text-align: center;"}


Title의 폰트를 변경하기 위해서는 인터페이스 빌더에서는 Title의 폰트변경을 위한 UI를 제공해 주지 않기 때문에 코드를 이용해야 합니다.

NSMenuItem은 Title에 Attributed String을 사용하는 것을 지원합니다.[^1]

예제 코드는 다음과 같습니다.

```objc
NSMenuItem *menuItem = [[NSMenuItem alloc] initWithTitle:@"Avenir Next" action:nil keyEquivalent:@""];
NSDictionary *attributes = @{ NSFontAttributeName: [NSFont fontWithName:@"Avenir Next" size:13.0] };
NSAttributedString *attributedTitle = [[NSAttributedString alloc] initWithString:[menuItem title] attributes:attributes];
[menuItem setAttributedTitle:attributedTitle];
```

![](https://farm2.staticflickr.com/1923/43233799160_ca75508955_o.png){: .center-image}
*Font Style이 적용된 Title*
{: style="text-align: center;"}


Title에 `AttributedString`을 `NSMenuItem`에 사용했을 때 한가지 제약사항은 정렬을 왼쪽정렬 밖에 할 수 없다는 것입니다.

이상으로 NSMenuItem Title에 폰트를 적용하는 법을 알아보았습니다.

[^1]: [https://developer.apple.com/reference/appkit/nsmenuitem](https://developer.apple.com/reference/appkit/nsmenuitem){:target="_blank"}