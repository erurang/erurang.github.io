---
layout: post
title:  "React Router"
subtitle: "React Router"
categories: antique
tags: antique
comments: true

---

## Router를 사용해보자

SPA의 단점은 한 페이지에 변경되는 구성요소만 불러와서 처리하는 것이기 때문에

히스토리를 남길수 없었다. 히스토리를 남기기 위해서 리액트에는 라우터라는것을 제공한다.

> npm i react-router-dom


### 라우터를 만드는 첫번째

```
<!-- App.js -->
import { BrowserRouter, Route, Switch } from "react-router-dom";

import Home from "./components/home";
import Profile from "./components/profile";

function App() {
  return (
    <BrowserRouter>
      <Switch>
      <!-- 아래에 exact={true} 추가 -->
        <Route path={["/", "/home"]}>
          <Home />
        </Route>
        <Route path="/profile">
          <Profile />
        </Route>
      </Switch>
    </BrowserRouter>
  );
}

export default App;
```
`path=["/","home"]` 이렇게 주었는데 이 url로 들어오면 `<Home />`이라는 동일한 라우터로 연결한다는 뜻이다.

home과 profile 컴포넌트는 다음과 같다.

```
<!-- home -->
import React from 'react';

const Home = (props) => (
    <>
        <h1>home</h1>
        <h1>Go to profile</h1>
    </>
);

export default Home;
<!-- profile -->
import React from 'react';

const Profile = (props) => (
    <>
        <h1>Profile</h1>
        <h1>Go to home</h1>
    </>
);

export default Profile;
```

각 라우터로 접속해보자

<img width="281" alt="스크린샷 2021-01-10 오후 7 55 00" src="https://user-images.githubusercontent.com/56789064/104120898-bbb2b280-537d-11eb-859b-e59a76e9b063.png">

<img width="285" alt="스크린샷 2021-01-10 오후 7 55 14" src="https://user-images.githubusercontent.com/56789064/104120902-c2d9c080-537d-11eb-89e6-766b89d4dfaa.png">

<img width="311" alt="스크린샷 2021-01-10 오후 7 59 20" src="https://user-images.githubusercontent.com/56789064/104120986-5612f600-537e-11eb-94a6-9284517b6046.png">

`/`  ㅇㅋ 잘되고 `home` ㅇㅋ 잘되고 `profile` ?? 얘는 왜 안돼지??

이건 리액트라우터가 처리하는 방식의 문제인데 제일먼저 선언된 / 를 보고 오케이 home을 부르자. 

그다음 url /home 이네 ㅇㅋ 너도 home을 부르자.

자 그다음 /profile / profile 어 너도 /가 들어가있네. home을 부르자

해서 뒤가 렌더가 되지 않는것이다. 이것을 해결하기위해 / 라우터에

`exact={true}`를 추가한뒤 다시 접속해보면

<img width="313" alt="스크린샷 2021-01-10 오후 8 08 58" src="https://user-images.
githubusercontent.com/56789064/104121176-adfe2c80-537f-11eb-880f-ec1ff4bba48f.png">

정상적으로 작동하는것을 볼수있다.

## 두번째로  `<Link to="url">` 을 쓰는 방법도 있다.

```
function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/profile">Profile</Link>
      </nav>  
    </BrowserRouter>
  );
}
```

![router](https://user-images.githubusercontent.com/56789064/104127557-13651400-53a6-11eb-94c0-684614032d4a.gif)


to에 설정한 url로 변경되는것을 볼수있다.

## 세번째로 버튼을 클릭했을때

버튼을 클릭했을때 다른 라우터로 이동을 하게 하려면  `useHistory`를 이용한다.

```
import React from 'react';
import { useHistory } from 'react-router-dom';

const Home = (props) => {
    const history = useHistory();
    return (

        <>
            <h1>home</h1>
            <button onClick={() => {
                history.push('/profile')
            }}>Go to profile</button>
        </>
    )
};

export default Home;
```

클래스 방식에선 component={} 방식도 있었지만 이것은 매번 렌더될때마다

새로운 컴포넌트를 만들기때문에 성능하락이 일어날수있다.

그래서 
```
<Route path="/profile">
          <Profile />
        </Route>
```

이렇게 부모자식 형태로 쓰는것이 좋다.

## HTML과 React Router의 차이점

HTML에서 `a(href="")`로 라우터를 이동할때는 모든 페이지가 전체 재생성 되지만

리액트라우터에서는 변화된 부분만 요소가 바뀌고 라우트가 변경된다.