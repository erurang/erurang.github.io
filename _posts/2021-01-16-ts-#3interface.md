---
layout: post
title:  "TypeScript interface"
subtitle: "TypeScript interface"
categories: web
tags: typescript
comments: true

---

## object에 type을 지정해서 넘겨주자.

이 전의 글에서 우리는 각 변수에 타입을 지정해서 넘겼다.

이것을 오브젝트 형식으로 넘겨줄순 없을까?

그래서 나온것이 `interface`이다.

<img width="723" alt="스크린샷 2021-01-16 오전 12 24 58" src="https://user-images.githubusercontent.com/56789064/104745300-47b74680-5791-11eb-87b0-f4bde3fb4c9a.png">

interface를 지정해서 `인자:인터페이스` 로 넘겨준다.

하지만 return을 보면 name, age, gender에 밑줄이 그여진걸 볼수있는데 

<img width="673" alt="스크린샷 2021-01-16 오전 12 29 24" src="https://user-images.githubusercontent.com/56789064/104745876-e643a780-5791-11eb-9cf7-7ecfcdc9b113.png">

우리가 넘겨받은 `변수.인자명` 처리를 하면 올바르게 컴파일이 되는것을 볼수있다.

인터페이스는 javascript에선 사용할수없다. 즉 Ts가 컴파일이 될때 js의 index.js를 보면 컴파일이 되어있지 않은걸 볼수있다.

## interface를 js에 표현하려면 class를 이용하자

`index.ts` 코드를 이렇게 변경해보자.
<img width="634" alt="스크린샷 2021-01-16 오전 12 57 18" src="https://user-images.githubusercontent.com/56789064/104749123-cb733200-5795-11eb-8af3-fb21fffe610e.png">

컴파일된 `index.js`에 class로 표현된것을 볼수있다.
<img width="666" alt="스크린샷 2021-01-16 오전 12 53 47" src="https://user-images.githubusercontent.com/56789064/104748640-4e47bd00-5795-11eb-982b-b4fbb43863cc.png">

여기서 class안에서 `public`과 `private` 2가지가 존재하는데.

기본적으로 `javascript`변수들은 `public`으로 지정되어있다.

`public`은 클래스 밖에서(외부에서) 객체의 멤버에 접근할수있고

`private`는 클래스 밖에서(외부에서) 객체의 멤버에 접근이 불가능하다.