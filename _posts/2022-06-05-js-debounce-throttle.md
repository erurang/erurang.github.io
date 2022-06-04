---
layout: post
title:  "Debounce & Throttle"
subtitle: "Debounce & Throttle"
categories: web
tags: javascript
comments: true

---

## 반복 호출을 제어하는 방법들

여러번의 반복 호출을 제어함으로써 얻는 이득은 무엇일까? 

자바스크립트에서 `DOM 이벤트`를 실행하는 성능을 조절할수도있다

만약에 `scroll event`가 일어날때마다 시간이 `1`초씩 걸리는 함수가 실행된다고 해보자.

사용자가 `scroll`을 한번 내려서 `scroll event`가 `10`번 일어났다고 가정해보자. `10`번 `10`초 정도는 괜찮을수도있다.

만약에 사용자가 `scroll`을 한번을 내리는게아니라 계속 내려서 `scroll event`가 `10000`번 발생한다고 가정해보자.

그렇다면 `10000`초가 걸리게된다. 즉 자바스크립트는 이 `scroll event`를 처리하기위해서 다른 이벤트를 처리하지 못하는 경우가 생기게된다.

또 다른 예시로 여러번의 중복된 `API` 호출을 막는 효과도 있다. 예를들어 사용자가 댓글을 작성후에 버튼을 눌러 처리하려고 한다.

사용자가 댓글작성버튼을 눌렀지만 서버에서 콜백이 느려 화면에는 적용이 안됬을수도있다. 그래서 사용자는 버튼을 몇번이고 누를수도있다.

그럼 서버에는 이전에 요청과 함께 사용자가 버튼을 누른 횟수만큼의 요청이 또 들어오게된다. 

또한 자동완성기능의 경우 사용자가 단어 하나를 칠때마다 서버에서 관련된 단어를 화면에 띄어준다고 해보자. `네이버` 이 3글자를 친다고할때

서버로 `ㄴㅔㅇㅣㅂㅓ` 6번 요청이 날라오게된다. 사용자가 입력을 끝낸후 일정시간뒤에 서버로 한번 요청을 보내서 자동완성 데이터를 받아오도록 하면 좋지않까?

이런 경우들을 방지하기위해서 호출을 제어하는 방법들이 필요했다. 즉 `Debounce & Throttle`은 이벤트가 과도하게 발생하는것을 막는 방법들이라고 할수있다.

## Debounce

![image](https://user-images.githubusercontent.com/56789064/172022119-53cf2146-ab4e-4ff2-9cd3-afbcaec81ce4.png)

이 그림에서 윗부분은 이벤트가 발생한 횟수이고 아랫부분은 이벤트가 실행된 횟수이다. 그림에서 알수있듯

이벤트가 최초 실행된후 이벤트의 발생이 멈춘 후 일정시간 후에 이벤트가 실행된것을 볼수있다. 

### Debounce 사용해보기

```
const debounce = (func, delay) => {
    let timer = null;

    return (...args) => {
        clearTimeout(timer)

        timer = setTimeout(func,delay)
    }
}
```

첫번째 인수로 실행하고자하는 함수를 받고, 지연되고자하는 시간을 적어준다. `debounce`의 원리는 `setTimeout`과 `clearTimeout`을 이용하는것이다.

첫 실행때 `setTimeout`으로 일정 `delay`뒤에 실행될 함수를 실행시켜준다. 그러고 만약 또 실행됫을시에 `clearTimeout`으로 이전의 `setTimeout`을 해제시키고

다시 `setTimeout`를 거는 방법이다. 

```
const debounce = (func, delay) => {
    let timer = null;

    return (...args) => {
        clearTimeout(timer)

        timer = setTimeout(func,delay)
}

const test = debounce(() => console.log('debounce test!'),1000)
```

<img width="409" alt="스크린샷 2022-06-05 오전 4 50 51" src="https://user-images.githubusercontent.com/56789064/172023505-7ca56bc9-df85-4ed6-bae7-242b4d815a9f.png">

`test()`를 수도없이 실행했지만 `console.log`는 실행되지않고 마지막에 딱 한번 실행되는걸 볼수있다.

즉 `debounce`는 자동완성기능, `API`호출에 유용하다

## Throttle

`Throttle`은 이벤트를 일정한 주기마다 발생하도록 하는 기술이다. 

예를 들어 `Throttle`의 설정 시간으로 `1ms`를 주게 되면 해당 이벤트는 `1ms` 동안 최대 한번만 발생하게 된다.

### Throttle 사용해보기

```
const throttle = (func, delay) => {
    let waiting = false

    return (...args) => {
        if(!waiting) {
            func(...args)
            waiting = true
            setTimeout(() => {
                waiting = false
            }, delay)
        }
    }
}

const test = throttle(() => console.log('throttle test'),1000)
```

<img width="450" alt="스크린샷 2022-06-05 오전 5 35 35" src="https://user-images.githubusercontent.com/56789064/172024728-978c3ae5-8eca-4bc8-921a-93856d39ea0b.png">

첫번째 인수로 실행하고자하는 함수를 받고, 실행되고자하는 시간을 적어준다. `setTimeout`으로 `waiting`이 변하는 시간만 고쳐준다.

그래서 첫 함수 호출때 한번 불려온뒤에 일정시간뒤에 `waiting`을 `false`로 수정하여 다시 실행될수있도록 만드는것이다.

## 정리 

`Throttle`은 적어도 `X` 밀리 초마다 실행을 하기를 원할때 사용을 하고,

`Debouce는` 많은 이벤트가 발생해도 모두 무시한채, 마지막 이벤트 발생뒤 일정시간 후, 딱 한번 마지막으로 실행된 이벤트를 발생시키는 방법이다.