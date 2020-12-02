---
layout: post
title:  "구체성"
subtitle: "구체성"
categories: web
tags: css
comments: true

---

구체성
---
선택자를  얼마나  명시적으로 선언했느냐를  수치화한  것으로,

구체성의  값이  클수록  우선으로  적용이 됨

```
0, 0, 0, 0
```
구체성은 4개의 숫자 값으로 되어 있는데

값을 비교할 때는 좌측에 있는 값부터 비교하며,

좌측 숫자가 클수록 높은 구체성을 가짐.


-   0, 1, 0, 0 : 선택자에 있는 모든 id 속성값
-   0, 0, 1, 0 : 선택자에 있는 모든 class 속성값, 기타 속성, 가상 클래스
-   0, 0, 0, 1 : 선택자에 있는 모든 요소, 가상 요소
-   전체 선택자(*)는 0, 0, 0, 0을 가진다.
-   조합자는 구체성에 영향을 주지 않는다. (>, + 등)
-  1, 0, 0, 0 : inline style일 경우

```
h1 { ... }  /* 0,0,0,1 */
body h1 { ... }  /* 0,0,0,2 */
.grape { ... }  /* 0,0,1,0 */
*.bright { ... }  /* 0,0,1,0 */
p.bright em.dark { ... }  /* 0,0,2,2 */
#page { ... }  /* 0,1,0,0 */
div#page { ... }  /* 0,1,0,1 */
```

!important
---

모든 구체성을 무시하고 우선권을 가짐

```
p#page { color: red !important; }

<p id="page" style="color:blue">Lorem impusm dolor sit.</p>
```

위의 ```<p>```에는 important로 인해 color: red가 적용됨.
