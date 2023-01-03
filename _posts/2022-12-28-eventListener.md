---
layout: post
title: "addEventListener는 해제가 되었을까? 해제가 되지않으면?"
subtitle: "addEventListener는 해제가 되었을까? 해제가 되지않으면?"
categories: web
tags: javascript
comments: true
---

# 이벤트가 해제되지않으면 다른곳에서 같은 이벤트를 호출할때 해제되지 않은 이벤트도 함께 작동한다.

웹뷰(리액트)에서 앱에 구현된 함수를 호출하여야 했다. 앱과 웹이 통신하는 방법중엔 `window`를 이용하는 방법이 있었다.

그래서 이벤트 등록하는 방법만 알고있었지 이벤트를 해제한다는 생각은 해본적이 없었다.

페이지를 이동할때마다 서로다른 이벤트가 같은 이름으로 묶여 이벤트가 여러번 일어나는 현상을 겪게 되었다.

이벤트를 추가/해제 할때의 주의사항과 이벤트 함수를 등록할때의 주의점에 대해 알아보자.

## React에서 앱함수를 호출 및 응답을 받기위한 예제코드

```
window.BRIDGE.hiAppFunc(); // kotlin
window.webkit.messageHandlers.hiAppFunc.postMessage(); // swift
```

내가 진행하던 프로젝트의 경우 앱 안드로이드/IOS 구분을 두어서 위처럼 호출을 하였다.

`window`는 브라우저의 `global` 변수이므로 프론트에서 만들어지지 않은 함수를 앱이 만든다.

그 후 `window`에 추가하면 리액트가 `window.function()` 실행을 할수 있게 된다.

타입스크립트의 경우 `react-app-env.d.ts` 파일에서 아래코드를 설정하지않으면 `window`에는 이 함수가 없다고 타입스크립트에서 경고를 주게된다.

`window`에는 `hiAppFun`라는 함수가 없으니 당연히 타입스크립트가 불평할만하다. 그래서 `window`에 우리가 사용할 인터페이스를 만들어 주어야한다.

```
interface Window {
  BRIDGE: {
    hiAppFunc : () => any;
  }
  webkit : {
    messageHandlers : {
      hiAppFunc : () => any;
    }
  }
}
```

이렇게 설정해 둔다면 이제 우리가 `hiAppFunc`함수를 호출하는데 타입스크립트는 아무런 불평을 하지않을것이다.

다음으로 이벤트를 들을 곳에서 이벤트를 들을수있도록 설정한 후 호출해주자.

```
window.addEventListener("hiAppFunc", () => console.log('hihi'))

window.BRIDGE.hiAppFunc()
window.webkit.messageHandlers.hiAppFunc.postMessage()
```

웹페이지가 `hiAppFunc`라는 이벤트를 듣도록 설정한다. 그 후에 `hiAppFunc()`를 실행하면 웹페이지는 `window`변수 안에 있는 `hiAppFunc`라는 이벤트를 실행하고

앱에서는 `hiAppFunc`라는 똑같은 이름으로 웹으로 이벤트를 던져준다. 그러면 웹은 이벤트를 듣고 우리가 설정한 함수인 콘솔로그가 작동하여 `hihi`가 표시되게 된다.

## 만약 이 이벤트를 해제하지 않으면 어떻게 되나?

리액트의 코드를 예제로 사용해보자. 우리에게 A페이지와 B페이지가 있다고 가정해본다. 두 페이지모두 이벤트를 듣도록 설정해두자. 앱에서 해당 이벤트에대해 모두 응답한다고 가정한다.

```
A page
useEffect(() => {
  getPrice()
  window.addEventListener("getPrice", () => console.log('Hi i'm A page'))
},[])

B page
useEffect(() => {
  getPrice()
  window.addEventListener("getPrice", () => console.log('Hi i'm B page'))
},[])
```

A페이지에 진입시에 "getPrice" 이벤트를 듣도록 설정한다. "getPrice"를 호출하였고 앱은 우리에게 응답을 해주므로써 웹은 `console.log('Hi i'm A page')`를 실행하게 된다.

자 B페이지로 이동해보자. B페이지 진입시에도 "getPrice" 이벤트를 듣도록 설정하였기 떄문에 "getPrice" 를 이벤트를 한번 더 듣게된다.

여기서 문제가 발생한다. 현재 웹에서 듣고있는 이벤트는 "getPrice" "getPrice" 2번이다. 즉 우리는 콘솔에 `Hi i'm A page` 와 `Hi i'm B page` 두개를 보게된다.

정말 웹은 이벤트를 두번 듣고있을까?? `getEventListeners(window)` 를 통해서 현재 웹이 어떤 이벤트를 듣고있는지 알수있다.

![스크린샷 2022-12-29 오후 2 21 58](https://user-images.githubusercontent.com/56789064/209906820-d17f513d-4e0b-4ccc-8075-ea68fdbcb270.png)

"getPrice(2)"로 표시된것이 보인다. 즉 앱은 `getPrice` 호출에 의해 응답을 한번 주었을뿐 웹에서 이벤트 해제를 해주지 않아서 A/B 페이지에서 설정한 함수가 실행됬을 뿐이다. 즉 우리는 이벤트 해제를 해주어야한다.

## 이벤트 해제해주기

이벤트를 해제해줄때는 꼭 removeEventListener도 중요하지만, 실행할 함수도 익명함수가 아니여야 한다! `React`에서 이벤트를 추가하고 삭제하기 위해서 `useEffect`를 사용하면 된다

```
function Event_APage() {
  console.log('Hi i'm A page')
}

function Event_BPage() {
  console.log('Hi i'm B page')
}

A page
useEffect(() => {
  getPrice()
  window.addEventListener("getPrice", Event_APage)

  return () => {
    window.removeEventListener("getPrice", Event_APage)
  }
},[])

B page
useEffect(() => {
  getPrice()
  window.addEventListener("getPrice",Event_BPage)

  return () => {
    window.removeEventListener("getPrice",Event_BPage)
  }
},[])
```

이렇게 하면 페이지를 이동할떄 이벤트가 해제되기때문에 페이지 이동시에 콘솔을 두번 보는 경우는 없어지게 된다.

## 이벤트를 만들때 주의할점.

만약 이벤트를 생성할때 이벤트가 실행될때 함수가 상태값을 사용하는경우라면!! 꼭 이벤트의 함수를 재설정 해줘야한다!!

그렇지않으면 컴포넌트에서 상태가 변경되어도 addEventListener를 통해 이벤트를 만들 때 그때의 상태값이 설정되어 함수에 저장되기 때문이다. (상태가 변한다고 함수안 상태가 변하지 않는다.)

### 예시

```
const [cnt,setCnt] = useState(0)

function Event_CountTest() {
  console.log('cnt :', cnt)
}

useEffect(() => {
  CountTest()

  window.addEventListener("getPrice", Event_CountTest)

  return () => {
    window.removeEventListener("getPrice", Event_CountTest)
  }
},[])

return (
  <div>
    <button onClick={() => setCnt(prev => prev+1)}>Count Plus</button>
    <button onClick={() => CountTest()}>Call Event_CountTest</button>
  </div>
)
```

위와 같은 리액트 코드가 있다고 해보자. `Count Plus` 10번 누르고 앱에서 만들어진 `CountTest()`를 통해 우리의 상태값을 가져오면 콘솔에는 0이 찍히게된다.

왜냐하면 `useEffect`를 통해 함수가 만들어진 그 순간 `cnt`의 값은 0이기 때문에 `Event_CountTest` 함수는 `console.log('cnt :",0)`으로 저장되기 때문이다

만약 이벤트로 일어날 함수가 상태변화에 영향을 받는다면 `useEffect`의 `deps`를 `[]`로 두는 것이 아닌 `[cnt]`로 두어 `cnt`에 변화가 일어날때마다 이벤트 함수를 재설정해 주어야 한다는 점을 잊지말자.
