---
layout: post
title:  "React Props"
subtitle: "React Props"
categories: antique
tags: antique
comments: true

---

## Props에 대해 알아보자.

앞에서 Component를 만드는 2가지 방식을 알아보았다.

이번엔 Component에 데이터를 주는 방법을 알아보자.

그 방법은 Props라고 한다.

부모컴포넌트에서 자식 컴포넌트에게 인자를 전달해주면

전달해준 인자들은 props라는 object로 묶여서 자식컴포넌트에 전달된다.

자식컴포넌트에서는 this.props로 참조가 가능하다.

사진으로 알아보자

<img width="476" alt="스크린샷 2021-01-04 오전 6 51 00" src="https://user-images.githubusercontent.com/56789064/103489625-3731d280-4e59-11eb-9dab-8f6eb9ab0054.png">

Parent라는 부모컴포넌트에서 Child 자식 컴포넌트에게 name과 func라는 인자를 전달해준다.

우리가 자바스크립트 문법을이용할때는 { } 를 사용해야한다.

그리고 클래스 내에 선언된 인자들은 this. 로 참조해야한다.

<img width="363" alt="스크린샷 2021-01-04 오전 6 55 47" src="https://user-images.githubusercontent.com/56789064/103489750-e1115f00-4e59-11eb-8fdc-1af445953c8e.png">

자식 컴포넌트에서는 this.props.인자명 으로 사용이 가능하다.

만약에 props로 전달을 하지 않을 경우엔 컴포넌트 안에서 

static defaultProps = {} 으로 선언이 가능하다.

<img width="1017" alt="스크린샷 2021-01-05 오후 10 01 11" src="https://user-images.githubusercontent.com/56789064/103649266-88e57480-4fa1-11eb-8ed2-dcef5b08d259.png">

<img width="340" alt="스크린샷 2021-01-05 오후 10 01 46" src="https://user-images.githubusercontent.com/56789064/103649328-9e5a9e80-4fa1-11eb-928d-4ffa7289a07b.png">

`<Navbar />`에서 어떤것도 전달받지 못햇지만 기본설정으로 지정이 가능하다. 

만약에 `<Navbar name="~"/>` 을 전달받는다면 전달한 요소가 기본설정을 덮어씌운다.

React에서 form의 data를 받아올때는 `React.createRef();`를 사용한다.
