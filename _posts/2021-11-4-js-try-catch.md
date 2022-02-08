---
layout: post
title:  "자바스크립트에서 Try Catch 에러 처리"
subtitle: "자바스크립트에서 Try Catch 에러 처리"
categories: web
tags: javascript
comments: true

---

## try catch를 쓰는 이유

예상치 못한 에러(잘못된 서버응답, 예상치못한 사용자입력... 등등)가 발생하면 스크립트는 죽고(중단되고) 콘솔에 `Error`가 출력되게 된다.

에러를 잡아서(catch) 스크립트가 죽는걸 방지하기위해 `try catch` 구문을 사용한다.

## try catch 문법

```
try {
  // 오류가 날수도 있다고 생각하는 코드
} catch(error) {
  // 에러핸들링
  // console.log(error)
} finally (선택사항 코드임) {
  // 실행/오류 와 상관없이 실행될 코드 
}
```

1. 먼저, `try` {...} 안의 코드가 실행
2. 에러가 없다면, `try` 안의 마지막 줄까지 실행되고, `catch` 블록은 건너뛰고 `finally` 실행
3. 에러가 있다면, `try` 안 코드의 실행이 중단되고, `catch(err)` 블록으로 제어 흐름이 넘어감
4. 변수 `error`(아무 이름이나 사용 가능)는 무슨 일이 일어났는지에 대한 설명이 담긴 에러 객체를 포함해 에러를 던진다.

## 사용해보자

### 에러가 없는 예시
```
try {

  alert('try 블록 시작');  // (1) <--

  // ...에러가 없습니다.

  alert('try 블록 끝');   // (2) <--

} catch(err) {

  alert('에러가 없으므로, catch는 무시됩니다.'); // (3)

}
```

1. `try`블록으로 들어가 1번을 실행 
2. 에러가 없기때문에 2번도 실행 
3. `try`의 모든 구문이 문제없이 실행되었기 때문에 `catch`가 실행되지않고 끝남

### 에러가 있는 예시
```
try {

  alert('try 블록 시작');  // (1) <--

  lalala; // 에러, 변수가 정의되지 않음!

  alert('try 블록 끝(절대 도달하지 않음)');  // (2)

} catch(err) {

  alert(`에러가 발생했습니다!`); // (3) <--

}
```

1. `try`블록으로 들어가 1번을 실행 
2. 에러가 존재하기 때문에 2번이 실행되지않고 `catch` 구문으로 넘어감
3. `catch`구문으로 넘어와 3번 에러를 발생시킴

### 에러 객체에는 어떤것이 넘어올까?

```
try {
  // ...
  balabla
} catch(err) { // <-- '에러 객체', err 대신 다른 이름으로도 쓸 수 있음
  alert(err.name); 
  alert(err.message); 
  alert(err.stack); 
}
```

의도적인 오류를 내어 에러객체로 어떤것을 받아올수있는지 테스트해보자.

### name

<img width="407" alt="스크린샷 2022-02-08 오후 4 19 32" src="https://user-images.githubusercontent.com/56789064/152937378-7f9ecc93-0202-4374-9cb4-6654ce403918.png">

에러이름. 정의되지않은 변수때문에 발생한 에러면 "ReferenceError" 가 뜬다.

### message
<img width="407" alt="스크린샷 2022-02-08 오후 4 19 37" src="https://user-images.githubusercontent.com/56789064/152937379-0b0962a9-d4df-4b45-b01d-e99acd1d10b6.png">

에러 상세내용을 담고있는 메세지

### stack
<img width="413" alt="스크린샷 2022-02-08 오후 4 19 40" src="https://user-images.githubusercontent.com/56789064/152937368-78e2d50e-56bd-4950-af8c-ce047388422d.png">

현재 호출 스택. 에러를 유발한 중첩 호출들의 순서 정보를 가진 문자열로 디버깅 목적으로 사용

### 정리 

`catch` 블록 안에서 새로운 네트워크 요청 보내기, 사용자에게 대안 제안하기, 로깅 장치에 에러 정보 보내기 등과 같은 구체적인 일을 할 수 있다.

아래의 실습을 통해 여러가지 에러를 관리해보자.

## 에러를 관리해보자.

### throw 연산자

```
throw <error object>
```

`throw`연산자는 에러를 생성한다. 일반적인 에러 핸들링 방법으로 아래 방법을 사용한다.

```
throw new Error("오류 발생시 나타날 메세지")
```

이렇게 에러가 던져지기만 하면 프로그램은 강제 종료가된다.

에러가 발생했을때 프로그램을 종료를 시키지 않고 오류를 받아서 처리하기 위해 `try catch`를 사용해보자.

## 실전 연습

### 에러를 던지고 프로그램이 종료되는 경우

```
function doException() {
  throw new Error('오류가 났어요~')
}

function main() {
  doException();
}

main()
```


![스크린샷 2022-02-08 오후 5 28 58](https://user-images.githubusercontent.com/56789064/152947734-d20257aa-1f19-480f-bcbb-8933992cb2c8.png)

이 경우 `throw`를 통해 에러를 던졌는데 아무곳에서도 `try catch`로 받아주지 않아서 프로그램이 그대로 종료가 된다.

### 에러를 받아와 처리한 경우

```
function doException() {
  throw new Error('오류가 났어요~')
}

function main() {
  try {
    doException();
  } catch(e) {
    console.log(e);
  } finally {
    console.log('나는 try구문이 끝나면 실행 돼')
  }
}

main()
```

![스크린샷 2022-02-08 오후 5 30 16](https://user-images.githubusercontent.com/56789064/152947989-e5eca040-f0b6-43c8-99fb-93fdacf448ef.png)

`throw`로 던져진 `Error`를 `catch`에서 받아서 처리를 해줬기때문에 프로그램이 종료가 되지않고 에러메세지를 출력해준걸 볼수있다.

### 만약 아래같은 코드에서 try catch를 어디서 써야할까?

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

만약에 위와같은 코드가 있다고 하자. `callException`에서는 `type`인자를 받아서 인자에 따라 다른 함수를 호출한다.

위의 코드에서 우리는 `try-catch`를 어디에 적용해야할까? 모든 함수에서 처리를 해줘야할까? 아니다.

`throw` 말그대로 에러를 던지는 개념이기 때문에 에러를 던지고나면 어느 위치(depth)에서든 상관없이 `catch`를 통해 오류를 받아오기만 하면 된다. 

그래서 위 코드에서 `try-catch`는 `main` 함수에만 적용하면 된다.

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