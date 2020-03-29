---
layout: post
title: "Data URI scheme이란?"
tags: 
  - 기본지식
comments: true
published: true
---

## 소    개

Data URI scheme은 이미지나 스타일 시트 등 외부 데이터를 URI(Uniform Resource Identifier)로 표현하는 방법입니다. 보통 HTML에서 이미지를 표현할 때, img 요소의 src 속성 값으로 이미지 경로를 입력하지만 Data URI scheme을 이용하면 Base64로 인코딩된 이미지 데이터를 직접 입력할 수 있습니다.

## 일반적인 HTML 이미지 표현

`<img src="http://kyejusung.com/hello.jpg">`


## DATAURI SCHEME을 이용한 HTML 이미지 표현

```html
<img src="data:image/png;base64,
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"/>
```

대부분의 웹브라우저와 이메일 클라이언트에서 Data URI scheme을 지원합니다.

## 포멧

`data:[<MIME-type>][;charset=<encoding>][;base64],<data>`

## 장단점

* 장점 : HTTP 요청과 헤더 트레픽이 발생하지 않고, 작은 파일이 여러 개인 경우 Data URI scheme을 이용하면 더 빨리 전송할 수 있습니다.

* 단점 : HTML문서에 첨부된 데이터 부분이 캐쉬되지 않는 점입니다. 또 Data URI scheme을 사용하면 데이터량이 1/3정도 증가하게 됩니다.

자주 사용하지는 않지만, 이기종 플랫폼 간의 이미지를 전송하고 바로 보여줄때 사용하면 편리할 것 같습니다.

## 참조

* [데이타 URI 스킴](http://hyeonseok.com/soojung/webstandards/2011/02/17/641.html){:target="_blank"}

* [위키피디아 “Data URI scheme”](https://en.wikipedia.org/wiki/Data_URI_scheme){:target="_blank"}