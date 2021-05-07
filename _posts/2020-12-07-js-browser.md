---
layout: post
title:  "Browser 구조와 API,DOM Tree "
subtitle: "Browser 구조와 API,DOM Tree"
categories: antique
tags: antique
comments: true

---

## Browser구조에 대해 알아보자.

웹페이지 화면을 2가지로 나눌수 있다.

<img width="1222" alt="스크린샷 2020-12-07 오후 12 24 00" src="https://user-images.githubusercontent.com/56789064/101305981-25273900-3887-11eb-9c7a-7346584ad8dd.png">

열려있는 전체 창을 window

페이지가 보이는 화면을 documents

전체적으로 browser에 관련된 navigator 가 있다.

<img width="466" alt="스크린샷 2020-12-07 오후 12 55 08" src="https://user-images.githubusercontent.com/56789064/101307709-720d0e80-388b-11eb-92dd-6755e690bd60.png">

## DOM (Document object model)

html파일을 브라우저가 읽고

브라우저는 각각의 html tag들을 분석해서 node로 만든다.

node들을 Tree로 만들어서 관리하며

이 node는 eventTarget의 object이다.

즉 모든 node는 이벤트가 발생할수 있다.

<img width="552" alt="스크린샷 2020-12-07 오후 6 58 05" src="https://user-images.githubusercontent.com/56789064/101336731-258ef680-38be-11eb-95ce-20adac2673be.png">

## CSSOM (CSS object model)

DOM + CSSOM 를 병합해서 Render Tree가 완성됨.

CSSOM에서 display : none 과 head tag의 내용들은 브라우저 화면에 표시가 되지 않기때문에

DOM + CSSOM으로 렌더트리가 만들어지면 Tree 해당 node는 생성이 되지않는다.

이렇게 만들어진 RenderTree가 사용되고 적용되는 자세한 방법은 [이곳](https://erurang.github.io/web/2020/12/08/js-render/)에 정리해 두었습니다.