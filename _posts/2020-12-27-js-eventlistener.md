---
layout: post
title:  "querySelector / all을 사용할때"
subtitle: "querySelector / all을 사용할때"
categories: web
tags: javascript
comments: true

---

## querySelector / all

mytube에서 댓글기능을 만들면서 생긴 중복오류로 인해

querySelector에 대한 개념을 다시 공부해보려한다..

내가 맞닥뜨린 오류가 무엇이냐..

나는 각 댓글이 생길때 수정/삭제 버튼에 addeventListner("click")을 등록하였다.

<img width="489" alt="스크린샷 2020-12-27 오후 10 34 31" src="https://user-images.githubusercontent.com/56789064/103172010-b0e32280-4893-11eb-8015-59f94696d50d.png">

위의 `handleEdit`는 콘솔에 `hi`라고 응답해준다.

자 이제 생긴 댓글에 수정버튼을 눌러보면..

![event](https://user-images.githubusercontent.com/56789064/103172153-d02e7f80-4894-11eb-8dd9-694bc2c22f2c.gif)

위버튼을 눌럿을때는 hi가 출력되는데 아래버튼을 눌럿을때는 hi가 출력이 안되는것을 볼수있다..

이게무슨일.. 그러니깐 이전에 생긴 댓글에는 수정/삭제 버튼을 눌러도 반응하지않고 새로 갱신된 댓글만 작동을 한다는 것이다.

이걸 해결하려면 어떻게 해야할까??

존재하는 모든 버튼을 다 선택해야한다. 그래서 querySelectorAll 을 사용해야한다.

위 코드를 `const CommentEditBtn = document.querySelectorAll()`이렇게 바꿔보자.

그럼 또 새로운 오류를 보게되는데

<img width="496" alt="스크린샷 2020-12-27 오후 10 53 29" src="https://user-images.githubusercontent.com/56789064/103172374-57c8be00-4896-11eb-89cf-221e700ebea2.png">

이게 무슨뜻이냐면 querySelectorAll은 단일 DOM을 돌려주는것이 아니고 여러개의 DOM을 돌려주기때문에 어떤것을 말하는지 모르는거다.

그래서 for문으로 `CommentEditBtn.length`만큼 `CommentEditBtn[].addEventListner("click")`를 사용해야한다.

```
const CommentEditBtn = document.querySelectorAll(".jsCommentEdit");

for (let i =0; i < CommentEditBtn.length; i++) {
    CommentEditBtn[i].addEventListener("click",handleEdit)
}
```

![event2](https://user-images.githubusercontent.com/56789064/103172550-a2970580-4897-11eb-8833-8a988a962c7f.gif)

아래댓글의 버튼을 눌러도 이제 hi가 출력되는것을 볼수있다!

