---
layout: post
title:  "가상클래스 & 가상요소"
subtitle: "가상클래스 & 가상요소"
categories: web
tags: css
comments: true

---

표준 가상클래스
---

미리 정의해놓은 상황에 적용이 되도록

약속되어있는 보이지않는 **클래스**


https://www.w3schools.com/css/css_pseudo_classes.asp
```
:pseudo-class
{
	property:value;
}
```

간단히 몇개만 알아보자.

```
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JS</li>
</ul>

li:first-child { color: red; }
li:last-child { color: blue; }
```

HTML과 JS에만 적용


```
a:link { color: blue; }
a:visited { color: gray; }
```
:link : 하이퍼 링크이면서 아직 방문하지 않은 앵커

:visited : 이미 방문한 하이퍼링크를 의미

```
a:focus { background-color: yellow; }
a:hover { font-weight: bold; }
a:active { color: red; }
```

:focus: 탭키를 눌럿을때 감싸는.. 

:hover: 마우스를 올렸을때

:active: a를 클릭할때나 button을 눌럿을때 처럼 순간적으로 활성화 됨.

가상요소
---

https://developer.mozilla.org/ko/docs/Web/CSS/Pseudo-elements

```
::pseudo-element 
{
    property: value;
    
    ---before,after---
    content: "추가할내용";
}
```
:before : 내용 가장 앞에 텍스트를 삽입 c

:after : 내용 가장 뒤에 텍스트를 삽입

:first-line : 요소의 첫 번째 줄에 있는 텍스트

:first-letter : 블록 레벨 요소의 첫 번째 문자