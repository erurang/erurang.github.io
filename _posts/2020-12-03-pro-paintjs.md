---
layout: post
title:  "그림판"
subtitle: "그림판"
categories: project
tags: paintjs
comments: true

---


![paintjs](https://user-images.githubusercontent.com/56789064/100940125-c1290b80-353a-11eb-95e1-2cb3806079d6.gif)

HTML의 canvas와 Js를 이용하여 만듬. 
[프로젝트 페이지](http://erurang.github.io/paintjs)
[소스코드 보러가기](https://github.com/erurang/paintjs)

모든 자료는 [공식문서](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas/Tutorial)에서 참조하였다. 


## app.js를 살펴보자

### [ 0 ] 캔버스 정의
엘리먼트는 고정 크기의 드로잉 영역을 생성하고 하나 이상의 렌더링 컨텍스(rendering contexts)를 노출하여, 출력할 컨텐츠를 생성하고 다루게 됩니다.

### [ 1 ] 캔버스 크기조절

#### **주의**
캔버스의 표시 크기는 CSS로도 수정할 수 있습니다. 그러나, CSS를 사용할 경우 렌더링 과정에서 CSS 크기에 맞추기 위해 이미지의 크기를 조절하므로, 최종 그래픽이 변형될 수 있습니다.

const canvas = document.getElementById("jsCanvas");
canvas.width = 700;
canvas.height = 700;

### [ 2 ] 캔버스 드로잉 컨텍스트 렌더링타입 정해주기

.getContext() 메소드는 캔버스의 드로잉 컨텍스트를 반환합니다.
2d, webgl(3d) 의 메소드가 있으나 여기선 2d로.

const ctx = canvas.getContext("2d");

ctx.lineWidth = 캔버스위에 그릴때 붓의 크기를 설정함

### [ 3 ] 캔버스 위에 그리기 테스트

ctx.fillStyle ="white"
ctx.fillRect(0,0,700,700);

위의 코드는 처음 canvas가 렌더될때 캔버스를 하얀색으로 초기화한것.
캔버스의 왼쪽 좌표로부터 위로 0,0 에 가로700 세로700 크기의 그림을 그린다는 뜻
그래서 채우기는 handleCanvasClick()은 화면전체를 칠함으로 만든것임.

### [ 4 ] 경로그리기

점들의 경로로 나타내어서 그리는것처럼 보이게함.
캔버스가 초기화 되었거나 beginPath() 메소드가 호출되었을 때, 
특정 시작점 설정을 위해 moveTo() 함수를 사용하는것이 좋습니다

const x = event.offsetX;
const y = event.offsetY; 둘다 마우스 커서가 움직일때의 좌표임.

moveTo(x,y) = 펜을 x와 y의 지정된 좌표로 옮기는 역할
lineTo(x,y) = moveTo에서 시작된 경로부터 lineTo의 x,y까지 선을 그림

ctx.beginPath() 로 경로의 시작을 알리고
ctx.moveTo(x,y) 로 x와 y의 시작점 을 알려준다

### [ 5 ] 색상 변경하기

fillStyle =  도형을 채우는 색을 설정합니다.
strokeStyle = 도형의 윤곽선 색을 설정합니다.

handleColorClick()은 

const color = event.target.style.backgroundColor; 
    => html에서 만든 Div 박스들의 backgroundcolor를 가져와서
ctx.strokeStyle = color;
    => 그리는 붓 색
ctx.fillStyle = color;
    => 채우는 색

### [ 6 ] addEventListner

mousemove 는 커서가 움직일때
mousedown 은 마우스왼쪽버튼을 누르고있을때
mouseup 은 누르고있던 버튼을 땔때

down상태라는건 누르고있다는 뜻이니깐 누를때만 beginPath가 시작되도록 정할수 있음.
그래서 onMouseMove(event) 에서 painting인지 아닌지 판별하고 그리게됨.