---
layout: post
title:  "code splitting을 위한 React lazy"
subtitle: "code splitting을 위한 React lazy"
categories: web
tags: react
comments: true

---

## 내 웹페이지는 왜이렇게 느린걸까?

로딩이 긴 이유를 찾기위해서 난 하나하나 파고들어봐야 했다. 일단 프론트에서 로딩이 길어진다..?

나는 어떻게 페이지 접속 로딩시간을 줄여 조금이라도 빨리 ui를 보여줄수있을까?

서버에서 데이터를 받아오는 시간이 길다는 뜻인데.. 이것을 줄이기위한 방법이 필요했다.

여기서 lazy를 사용할 이유를 알게되었다.

## 리액트에서 웹페이지를 빠르게 로딩하려면..?

1. 웹페이지가 빌드가 되면, 하나의 html파일에 모든 js파일이 합쳐지게된다.
2. 빌드된 웹페이지에 우리가 접속한다.
3. html에 적힌 모든 js를 다운로드한후 실행한다.
4. 완성된 웹페이지를 우리가 이용한다.

사용자가 우리 웹페이지를 열면 페이지의 내용물들(html,js,img,css...)을 다운받는다. 리액트의 경우 js로 ui를 만들기 때문에

js파일을 얼마나 빨리 받아서 ui를 만드느냐! 이것이 조금이라도 ui를 빠르게 보여주는 방법이라고 할수있다.

우리는 사용자가 굳이 보지않을것 같은 페이지의 js까지 페이지 접속시에 받을 필요는 없다. (adminpage.. 등등)

lazy를 쓰지않은 리액트는 모든 페이지에서 필요한 js파일을 다운로드하기때문에 시간이 길어질수밖에없다. 

이것을 나누기위해 react.lazy()가 등장한것이다.

## lazy() 사용해보기

```
import { lazy, Suspense } from "react";

// lazy(() => import('페이지 경로'))

const LazyComponent = lazy(() => import("./components/lazy"));
const AComponent = lazy(() => import("./components/a"));


<Suspense fallback={<h1>로딩중 ...</h1>}>
    <LazyComponent />
    <AComponent />
</Suspense>
```

일단 react 에서 lazy와 suspense를 import 해온다. 그 후 lazy를 적용하고싶은 페이지를 적용시킨다.

suspense를 통해 lazy컴포넌트를 감싸준다. fallback에서는 lazy컴포넌트를 받아오는 동안 (js파일을 받아오는동안) 보여줄 컴포넌트를 지정해준다.

이제 우리가 지정한 2컴포넌트는 js파일을 받아오는 동안 `로딩중...` 이라는 글자가 화면에 나타나게 된다.

모든 js파일이 아닌 그때 필요한 js만 다운로드 받는것, 이것이 lazy를 이용한 code splitting 이다.

## lazy를 사용하지 않았을때 빌드

![스크린샷 2021-12-16 오후 5 15 06](https://user-images.githubusercontent.com/56789064/152797060-ccea9143-abb3-4856-a2dc-d82a49b8d7b1.png)

<img width="380" alt="스크린샷 2021-12-15 오전 8 49 38" src="https://user-images.githubusercontent.com/56789064/152801382-461c89c5-d325-4d35-8680-01f9b9ea413b.png">

Lazy를 사용하지않았을때는 main.chunk.js에서 모든 js파일이 합쳐져서 다운받아 오는것을 볼수있다.

이것을 lazy로 나눈다면 아래처럼 바뀌게된다.

## lazy를 사용했을때 빌드

![스크린샷 2021-12-16 오후 5 12 42](https://user-images.githubusercontent.com/56789064/152800325-38269667-9030-413f-a592-77f2d0f072c8.png)

<img width="468" alt="스크린샷 2021-12-16 오후 5 10 04" src="https://user-images.githubusercontent.com/56789064/152797063-201aa141-eb23-4ab8-b818-1101bd0b3e40.png">

lazy를 사용해 여러 컴포넌트를 chunk로 나누었다. 그래서 우리는 꼭 필요한 페이지에 접속한경우에 chunk파일들을 따로 받아와 ui를 좀더 빠르게 실행할수있다.
