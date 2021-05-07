---
layout: post
title:  "버블링 캡처링 이벤트 위임"
subtitle: "버블링 캡처링 이벤트 위임"
categories: antique
tags: antique
comments: true

---

## 버블링 캡처링 이벤트위임에 대해 알아보자.

querySelector를 공부하다가 쿼리셀렉터는 한 요소에만 적용된다는걸 알았고

All 은 모든 요소에 적응되어 DOM형태로 돌려준다는것을 알았다.

[쿼리셀렉터](https://erurang.github.io/web/2020/12/27/js-eventlistener/) 여기에서 정리한대로

forEach를 각 요소에 이벤트리스너로 처리해도 되지만 이번 mytube 프로젝트를 진행하면서 코드를 이렇게 바꿨다.

```
if (addCommentForm) {
  const addCommentForm = document.getElementById("jsAddComment");
  const jsCommentUser = document.getElementById("jsCommentUser");

  addCommentForm.addEventListener("submit", handleSubmit);
  jsCommentUser.addEventListener("click", handleComment);
}
```

가장 최상위 박스를 jsCommentUser에 이벤트리스너를 하나만 달아둔다.

이제 그 박스안의 요소를 클릭할때마다 event가 생성이 된다.
우린 그중에서 필요한 event.target을 찾아 골라서 원하는 이벤트를 처리하면 된다.


```
const handleComment = (event) => {
  // 댓글 수정
  if (event.target.parentNode.className == "jsCommentEdit") {
  }

  // 댓글 삭제
  if (event.target.parentNode.className == "jsCommentDelete") {
  }

  // 댓글 좋아요
  if (event.target.id == "jsCommentLike") {
  }

  // 댓글 싫어요
  if (event.target.id == "jsCommentunLike") {
  }
};
```

위의글을 왜 적었을까? 내가 이용한것이 이벤트 버블링이다.

## 버블링

한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작함.
가장 최상단의 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작함.

```
<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```
![image](https://user-images.githubusercontent.com/56789064/103324992-08001780-4a8d-11eb-9b97-c90e488cf4e3.png)

즉 여기서 p를 누르면 div form 까지 모두 onclick이벤트가 발생하는걸 알수있다.

여기서 시작점만 고르기위해서 우린 event.target을 이용한다.

## 캡처링

<img width="680" alt="스크린샷 2020-12-30 오전 10 58 22" src="https://user-images.githubusercontent.com/56789064/103325181-f0755e80-4a8d-11eb-9512-ca238d30d8f2.png">

버블링과 반대로 캡처링은 최상단부터 타겟까지 내려오는 과정을 말한다.

캡처링 -> 버블링 으로 이벤트가 실행된다고 할수있다.

## 이벤트위임

우린 위의 개념들을 토대로 발생시키고자 한 시점(event.target)만 이벤트 리스너를 등록해두고

안의 요소들은 event.target 안의 요소들로 접근하여 이벤트를 처리할수있다.

1. 컨테이너에 하나의 핸들러를 할당한다.
2. 핸들러의 event.target을 사용해 이벤트가 발생한 요소가 어디인지 알아낸다
3. 원하는 요소에서 이벤트가 발생했다고 확인되면 이벤트를 이용함

즉 각각의 요소에 이벤트리스너를 등록하지않고 컨테이너 하나에만 이벤트리스너를 해둠으로써 장점은

1. 많은 핸들러를 할당하지 않아도 되기 때문에 초기화가 단순해지고 메모리가 절약됨
2. 요소를 추가하거나 제거할 때 해당 요소에 할당된 핸들러를 추가하거나 제거할 필요가 없다
3. DOM(코드) 수정이 쉬워진다.
