---
layout: post
title:  "프로모션 페이지"
subtitle: "프로모션 페이지"
categories: project
tags: uiux
comments: true

---

![promotionpage](https://user-images.githubusercontent.com/56789064/90402884-13086b80-e0db-11ea-9ab0-b34c52e4d402.png)

https://www.edwith.org/boostcourse-ui/lecture/47664/

강의를 토대로 만들면서 공부할점과 의문점을 정리합니다.

위의 사이트를 만들기위해서 큰 틀을 먼저 잡아야한다.

**상단 제목태그**
```
<h1>알면 알수록 즐거운 네이버쇼핑 윈도시리즈</h1>
<p>쇼핑 즐기고 10%적립 혜택도 받으세요!</p>
```

**메인을 리스트로 나타내자**
```
<ol>
<li>
	<strong>100 포인트만 있으면!</strong>
	<p>포인트 100원 이상 보유 시 신청 가능(신청 확인용으로 차감되지 않습니다.)</p>
	<a href="#">포인트가 부족하다면?</a>
</li>
<li>
	<strong>쇼핑윈도 시리즈 즐기고~</strong>
	<p>11.15 ~ 11.30 누적 1만원 이상 구매</p>
</li>
<li>
	<strong>구매금액 10%적립</strong>
	<p>최대 1만원 네이버페이 포인트 적립</p>
</li>
</ol>
```

**하단의 신청하기버튼**
```
<a href="#">신청하기</a>

<h2>유의 사항</h2>
<ul>
    <li>BIG3 쇼핑 이벤트와 중복으로 신청하실 수 없습니다.</li>
    <li>내부 사정에 따라 공지 없이 신청 종료될 수 있습니다.</li>
</ul>
```

**배경화면**

우리는 이미지를 배경으로 쓸것이다.

```
.wrap {
	position: relative;
	width: 1020px;
	height: 650px;
	margin: 0 auto;
	background: url(img/promotion.png) no-repeat 0 0;
}
```
width와 height를 주는 이유는 이미지를 배경으로 쓰는데

html안에서의 class="wrap"을 쓰는 태그의 width와 height가

이미지보다 작을경우 이미지가 짤리기때문에

이미지의 크기만큼 .wrap{}에서 width와 height를 재지정해준다.


하지만 우리는 이미지를 쓸것이기 때문에

적은 텍스트들을 화면에서 숨겨야 한다.

그래서 blind라는 class를 만들어 css에서 처리한다.

```

.blind {
	position: absolute;
	clip: rect(0 0 0 0);
	width: 1px;
	height: 1px;
	margin: -1px;
	overflow: hidden;
}
```

**absolute를 지정후**

![absolute](https://user-images.githubusercontent.com/56789064/90404480-6aa7d680-e0dd-11ea-8b0e-df0f5f507873.png)

absolute를 지정했기때문에 안의 요소들은

display block 형태로 변하고 absolute의 위치를 정할

relative가 필요하다. 상위 부모를 class="wrap"으로 지정하여

blind가 wrap에 absolute되도록 조정을 한다.

그리고 clip:rect(0 0 0 0); 을 통해 화면밖으로 보내준다.

그럼 화면에

```
1.
```
만 남게되는데 이것은 ol의 숫자매김번호이다

css에서 body와 ol도 초기화를 해주자.

```
ol {
	list-style: none;
}

body, ol {
	margin: 0;
	padding: 0;
}
```

모든 코드는 이렇다.

html
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>네이버 쇼핑 프로모션</title>
    <!-- tpye="text/css"가 뭔지 공부-->
    <link rel="stylesheet" type="text/css" href="css/promotion.css" />
  </head>
  <body>
    <!-- 이미지와 내용을 1:1로 매칭해주는게 가장 일반적인 방법 -->
    <!-- 이미지하나에 숨김텍스트하나 -->
    <!-- css에서 background를 처리하기위해 div로 만듬 -->
    <div class="wrap">
      <!-- css에서 숨김처리를 하기위해 blind클래스를 지정했고
        div안에 숨겨야할 텍스트를 적어도되지만 
        여기서는 숨겨야할 태그마다 class="blind"를 주기로함-->
      <h1 class="blind">알면 알수록 즐거운 네이버쇼핑 윈도시리즈</h1>
      <p class="blind">쇼핑 즐기고 10%적립 혜택도 받으세요!</p>
      <!-- 총 리스트를 OL로 잡아서-->
      <ol>
        <!-- 맨왼쪽 -->
        <li>
          <strong class="blind">100 포인트만 있으면!</strong>
          <p class="blind">
            포인트 100원 이상 보유 시 신청 가능(신청 확인용으로 차감되지
            않습니다.)
          </p>
          <!-- 모두 화면에서 숨겨놨기 때문에 link_point로 작업 -->
          <a href="#" class="link_point"
            ><span class="blind">포인트가 부족하다면?</span></a
          >
          <!-- 앵커태그는 살려야하니깐 strong p에 각각 blind를 주고
        살릴필요가 없는곳 li는 전체를 class="blind" 처리함  -->
        </li>
        <!--중간 -->
        <li class="blind">
          <strong>쇼핑윈도 시리즈 즐기고~</strong>
          <p>11.15 ~ 11.30 누적 1만우너 이상 구매</p>
        </li>
        <!-- 끝 -->
        <li class="blind">
          <strong>구매금액 10%적립!</strong>
          <p>최대 1만원 네이버페이 포인트 적립</p>
        </li>
      </ol>
      <a href="#" class="link_join"><span class="blind">신청하기</span></a>
      <h2 class="blind">유의사항</h2>
      <!--왜 여기는 UL이고 위는 OL인지 체크-->
      <ul class="blind">
        <li>BIG3 쇼핑 이벤트와 중복으로 신청하실 수 없습니다.</li>
        <li>내부 사정에 따라 공지 없이 신청 종료될 수 있습니다</li>
      </ul>
    </div>
  </body>
</html>

```
css
```
@charset "utf-8";

/* list에 매김숫자 빼기위한 리셋 */
ol {
  list-style: none;
}

body,
ol {
  margin: 0;
  padding: 0;
}
.wrap {
  position: relative;
  /* width와 height를 넣지않으면 body크기에 비해 img가 더 커서 img가 짤림. 그 때문에 이미지 크기만큼 지정함*/
  width: 1020px;
  height: 650px;
  /* 중앙정렬을 위해 */
  margin: 0 auto;
  background: url(../img/promotion.png) no-repeat 0 0;
}

/* blind라는 클래스에 숨긴텍스트 관련 속성들을 넣어줌 */
.blind {
  position: absolute;
  clip: rect(0 0 0 0);
  width: 1px;
  height: 1px;
  margin: -1px;
  overflow: hidden;
}

/* 숨김처리한 a태그 */
.link_point {
  position: absolute;
  left: 127px;
  bottom: 195px;
  width: 97px;
  height: 14px;
}

.link_join {
  position: absolute;
  bottom: 114px;
  left: 311px;
  width: 272px;
  height: 57px;
}

```