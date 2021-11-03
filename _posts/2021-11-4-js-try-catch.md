---
layout: post
title:  "자바스크립트에서 Try Catch 에러 처리"
subtitle: "자바스크립트에서 Try Catch 에러 처리"
categories: web
tags: javascript
comments: true

---

실습을 통해서 에러처리방식을 익혀보자. Error를 던질떄는 이렇게 사용한다.

```
throw new Error("오류 발생시 나타날 메세지")
```

이렇게 에러가 던져지고나면 프로그램은 강제 종료가된다. 그럼 이 오류가 발생했을때 종료를 시키지 않고 오류를 받아서 처리할방법은 무엇이 있을까?

try-catch를 통해 프로그램 종료없이 오류 핸들링을 할수있다. 사용방식은 아래와같다.

```
try {
    // 실행할 코드
} catch(error) {
    // 코드실행후 오류가 난다면 처리할 코드
} finally {
    // 코드의 실행/오류발생과 상관없이 항상 작동되는 코드
}
```

아하~ try catch를 통해서 오류핸들링을 할수있구나! 어.. 그런데..? 만약 아래같은 코드에서 try catch를 어디서 써야할까?

```
function doException() {
    // 여기??
  throw new Error("doException");
}

function noException() {
    // 여기??
  throw new Error("noException");
}

function callException(type) {
  if (type === "do") {
      // 여기??
    doException();
  } else {
      // 여기??
    noException();
  }
}

function main() {
    // 여기??
    callException("do");
}

main();
```

1. 최상위 함수인 main 안에서 한번?
2. main doException noException callException 모든 함수 안에서 매번?
3. callException에서 한번?


여러가지 많은 경우의수가 있겠지만, 답은 1번이다 throw 말그대로 에러를 던지는 개념이기 때문에 에러를 던지고나면

어느 위치에서든 상관없이 catch를 통해 오류를 받아오기만 하면 된다. 그래서 위코드의 main함수를 이렇게 고쳐서 사용해야한다.

```
function main() {
  try {
    callException("do");
  } catch (error) {
    console.log(error);
  } finally {
    console.log("done");
  }
}
```

위와같은 try-catch를 통한 에러핸들링 방식으로 오류가 발생시에 잡아내어 처리할수있다.