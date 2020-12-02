---
layout: post
title:  "웹UI 프로젝트A-1"
subtitle: "웹UI 프로젝트A-1"
categories: project
tags: boostcourse
comments: true

---

![projecta-1](https://user-images.githubusercontent.com/56789064/92482713-e4396d00-f222-11ea-9e8c-fcd9d48795fd.jpg)


https://www.edwith.org/boostcourse-ui/project/42/content/35#summary 

~~보이는건 똑같이 만들었다.~~

~~코드리뷰에서 지적이 예상될 곳이 몇군데가 있다.~~

~~1. menu클래스에서 width가 1000px인데 여기를 계산하지않고 야매로 맞춘점~~

~~2. header에서 포트폴리오 라고 적힌부분을 내 임의로 witdh값을 줘서 야매로 가로정렬을한점~~

~~솔직히 이거 2개말고는 큰 문제 없었던거 같다.~~

~~하지만 프로젝트A-2를보니 아우.. 만드는데 몇시간은 걸릴거같더라.~~

~~리뷰결과가 나오는대로 고칠점을 반영하겠다.~~

---
9월11일에 리뷰가 등록이 되었다.

문제점이 몇가지가 있었다.

1.  div같은 태그로 CSS에서 선언하지말고 class를 이용하라.
2. margin: 0 auto; 는 고정된 width px에서 만 사용이 가능하다. 
3. 테이블 / 폼 요소 / 리스트 요소 / 버튼에 클릭링크를 박스전체로

1번이 가장 중요한것 같다.

간단히 생각해서 div안에 요소가 별로없을때 :last-child 이런식으로 처리하곤 했는데

클래스를 선언해서 처리해주는게 유지보수에 도움이 된다고 한다.

적용 제출 후 PASS.