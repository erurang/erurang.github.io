---
layout: post
title:  "Redux를 사용하였을때"
subtitle: "Redux를 사용하였을때"
categories: web
tags: redux
comments: true

---

### 리덕스를 사용하였을때 코딩

```
<html>
    <head>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.1.0/redux.js"></script>
    </head>
    <body>
        <style>
            .container {
                border:5px solid black;
                padding: 10px;
            }
        </style>
        <div id="red"></div>
            <script>
                const reducer = (state,action) => {
                    console.log(state,action)
                    // 1. 처음 초기값을 지정하는 단계
                    if(state === undefined) {
                        return {color: "yellow"}
                    }
                }
                // 스토어 생성
                // 1. 스토어가 생성되었을때 초기값을 넣어줘야함
                const store = Redux.createStore(reducer);
                
                // 2. 현재 스토어의 상태를 가져오기
                console.log(store.getState())
                
                function red() {
                    // 3. style="background-color:${store.getState().color}" 추가하여 현재의 상태값을 기본값으로 지정해본다.
                    document.querySelector('#red').innerHTML= `
                    <div class="container" id="component_red" style="background-color:${store.getState().color}">
                        <h1>red</h1>
                        <input type="button" value="fire" onclick="
                        document.querySelector('#component_red').style.backgroundColor = 'red'/>
                        "
                    </div>
                    `
                }
                red();
            </script>
    </body>
</html>
```

### store 생성과 초기값지정, getState()

1번에서 store를 생성후에 reducer에 초기값을 지정해준 후, 2번을 통해 현재 스토어의 상태를 가져와보자.

`{color:"yellow"}` 가 콘솔에 찍히는걸 볼수있다. 초기값이 지정되었으니. 우리는 3번과정으로 `getState()`를 통해 얻어온 color 를 기본값으로 지정해보자.

![스크린샷 2021-05-05 오후 11 53 51](https://user-images.githubusercontent.com/56789064/117161748-27250200-adfd-11eb-9955-cc887b2b48f2.png)

store에서 얻어온 기본 상태값으로 노란 배경색이 지정된것을 볼수있다. 자 우리는 스토어를 만들고 스토어에서 상태값을 받아오는것 까지는 해보았다.


### dispatch() 와 action

다음으로 우리가 할것은 상태가 변화됩니다! 하고 스토어에게 알리는 dispatch에 대해 알아보자. 기존의 코드를 아래와같이 변경해보자

```
<input type="button" value="fire" onclick="document.querySelector('#component_red').style.backgroundColor = 'red'"/>
=> <input type="button" value="fire" onclick="store.dispatch({type:'CHANGE_COLOR', color:'red'});"/>
```

dispatch를 호출을하면 reducer로 우리가 지정한 값들을 넘겨준다. store안에 `console.log(state,action)`를 두어 확인해보자.

![스크린샷 2021-05-06 오전 12 08 21](https://user-images.githubusercontent.com/56789064/117164016-2e4d0f80-adff-11eb-8bdc-719b94a6a69a.png)

1번째줄은 스토어가 처음 최초로 생성될때의 값, 2번째 줄은 우리가 초기값으로 지정한 값, 3번째 줄은 현재의 store상태, dispatch로 넘긴 값 2가지를 볼수있다.

우리는 dispatch를 사용해 reducer의 action인자에 우리가 원하는 값이 들어간것을 위의 3번째 출력으로 확인했다. 그럼 이번엔 값을 받아서 스토어의 상태를 변화시켜보자

```
const reducer = (state,action) => {
    console.log(state,action)

    // 리턴할 상태를 지정
    let newState;

    // 1. 처음 초기값을 지정하는 단계
    if(state === undefined) {
        return {color: "yellow"}
    }

    if(action.type === 'CHANGE_COLOR') {
        // 현재의 state를 변경해서 리턴하는거보다 현재의 스테이트를 복사후 변경점만 리턴하는것이 올바른 방법임.
        // 이유는 불변함/성 을 지켜줘야하기때문
        // 객체를 복사할건데 ( {} 빈 객체에 , 뒤의 값을 복제하겠습니다 ) 라는 뜻

        newState = Object.assign({},state, {color : 'red'})
    }

    return newState
}
```

![스크린샷 2021-05-06 오전 12 21 39](https://user-images.githubusercontent.com/56789064/117166112-09f23280-ae01-11eb-8dc3-4b6898d57809.png)

우리가 dispatch를 통해 reducer에게 상태변화를 알렸고, action으로 store의 state를 yellow에서 red로 변화시켰다. 

**하지만 store내에서 상태를 변경할때 state.color = "red" 이렇게 바꾼것이 아니라** 

**Object.assign({},state,변경점)을 이용한 객체복사로 기존 상태를 복사하고 새로운 상태를 리턴해준것을 볼수있는데**

이것을 불변성이라고 한다. 불변성에 대해선 [불변성이란 무엇일까](https://erurang.github.io/web/2021/05/06/js-immutability/) 이곳에 정리해두었다. 

스토어의 state가 변화됬지만 요소에 상태변화가 (빨간색으로) 일어나지는 않은것을 볼수있다.

다음으론 상태가 변할때마다 render가 통보받아서 상태변화를 시켜줘야한다.

### subscribe()

그래서 상태가 변할때마다 render를 시켜주기위해 store에는 subscribe()가 있다.

`store.subscribe(상태변화가 일어났을때 실행하고자하는 함수)` 를 사용으로 상태변화가 일어날때 같이 변하는것을 볼수있다.

총 코드는 아래와같다.
```
<html>
    <head>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.1.0/redux.js"></script>
    </head>
    <body>
        <style>
            .container {
                border:5px solid black;
                padding: 10px;
            }
        </style>
        <div id="red"></div>
        <div id="blue"></div>
            <script>
                const reducer = (state,action) => {
                    console.log(state,action)

                    let newState;

                    // 1. 처음 초기값을 지정하는 단계
                    if(state === undefined) {
                        return {color: "yellow"}
                    }
                    if(action.type === 'CHANGE_COLOR') {
                        // 현재의 state를 변경해서 리턴하는거보다 현재의 스테이트를 복사후 변경점만 리턴하는것이 올바른 방법임.
                        // 이유는 불변함/성 을 지켜줘야하기때문
                        // 객체를 복사할건데 ( {} 빈 객체에 , 뒤의 값을 복제하겠습니다 ) 라는 뜻
                        newState = Object.assign({},state, {color : action.color})
                    }

                    return newState
                }
                // 스토어 생성
                // 1. 스토어가 생성되었을때 초기값을 넣어줘야함
                const store = Redux.createStore(reducer);
                
                // 2. 현재 스토어의 상태를 가져오기
                console.log(store.getState())
                
                function red() {
                    // 3. style="background-color:${store.getState().color}" 추가하여 현재의 상태값을 기본값으로 지정해본다.
                    document.querySelector('#red').innerHTML= `
                    <div class="container" id="component_red" style="background-color:${store.getState().color}">
                        <h1>red</h1>
                        <input type="button" value="fire" onclick="
                            store.dispatch({type:'CHANGE_COLOR', color:'red'});
                        "/>
                    </div>
                    `
                }
                red();

                function blue() {
                    // 3. style="background-color:${store.getState().color}" 추가하여 현재의 상태값을 기본값으로 지정해본다.
                    document.querySelector('#blue').innerHTML= `
                    <div class="container" id="component_blue" style="background-color:${store.getState().color}">
                        <h1>blue</h1>
                        <input type="button" value="fire" onclick="
                            store.dispatch({type:'CHANGE_COLOR', color:'blue'});
                        "/>
                    </div>
                    `
                }
                blue();

                store.subscribe(red)
                store.subscribe(blue)
            </script>
    </body>
</html>
```

실행하면 우리가 리덕스를 사용하기전과 같이 작동하는것을 볼수있다.

![a](https://user-images.githubusercontent.com/56789064/117170569-18424d80-ae05-11eb-96b0-6607cf276f9b.gif)


### 정리

리덕스를 사용하기 전을 생각해보자. 모든 요소마다 다른 요소들의 정보를 가진 의존관계였다. 

만약에 수정/추가/삭제 시에 모든 요소에 수정사항을 일일이 다 적어주어여 한다.독립적인 요소가 아니라고 할수있다.

리덕스라는 중앙 관리소에 우리의 모든 상태를 저장하여 요소간의 상태를 신경쓸 필요가 없어져 의존관계에서 벗어낫다. 독립적인 요소가 되었다고 할수있다.

하지만 리덕스를 사용하기위한 reducer의 action들을 하나하나 지정하다보니 코드의 수가 늘어났다고도 생각할수있다.

요소가 적을경우 리덕스를 굳이 사용하지않아도 되지만 상태를 관리해야할 요소가 많아질경우엔 리덕스를 도입하는것을 생각해보아야 한다.