---
layout: post
title:  "React Children과 Props"
subtitle: "React Children과 Props"
categories: antique
tags: antique
comments: true

---

리액트에선 Props라는 것이 있다는것을 [이전글](https://erurang.github.io/web/2021/01/04/web-react-props/)에서 포스팅한 적이 있다.

조금 더 쉽게 위의 내용들을 풀어보려한다.

1. 일단 App이 Hello컴포넌트를 그릴수 있도록 새 파일을 만들어주자.

- App.js

```
import React from "react";
import Hello from "./Hello";

import "./app.css";

function App() {
  return (
    <>
      <Hello />
    </>
  );
}

export default App;
```

- Hello.js

```
import React from "react";

function Hello({ name }) {
  return (
    <>
      <div>안녕 {name}</div>
    </>
  );
}

Hello.defaultProps = {
  name: "난 defaultProps",
};

export default Hello;
```

defaultProps는 Props로 넘겨받은게 없을때 기본값을 정해주는것이라고 이전글에서 어렵게 설명해놓았다.

현재 위의 App.js에는 `<Hello />`밖에 없으므로 우리가 name을 넘겨준것이 없고 defaultProps를 만들어주었다.

출력결과는 아래와 같다.

<img width="150" alt="스크린샷 2021-02-14 오후 2 59 56" src="https://user-images.githubusercontent.com/56789064/107869789-4e8ec180-6ed5-11eb-9dd9-297c5b562e59.png">

이번엔 name을 Props로 넘길수있도록 `<Hello />` 밑에 `<Hello name="props에서 왔어요" />`를 추가해보자.

<img width="157" alt="스크린샷 2021-02-14 오후 3 01 21" src="https://user-images.githubusercontent.com/56789064/107869817-81d15080-6ed5-11eb-9f85-ddd3ae9842c6.png">

그럼 props로 name을 넘겨받았기때문에 이제는 넘긴값이 올바로 나오는것을 볼수있다.

1. 이번엔 children을 배워보자.

Wrapper.js를 만들어주자

```
import React from "react";

function Wrapper() {
  const style = {
    padding: "20px",
    border: "2px solid black",
  };
  return <div style={style}></div>;
}

export default Wrapper;
```
App.js를 아래와 같이 변경해주자.

```
import React from "react";
import Hello from "./Hello";

import "./app.css";
import Wrapper from "./Wrapper";

function App() {
  return (
    <Wrapper>
      <Hello />
      <Hello name="props에서 왔어요" />
    </Wrapper>
  );
}

export default App;
```

결과를보면.. 
<img width="458" alt="스크린샷 2021-02-14 오후 3 04 45" src="https://user-images.githubusercontent.com/56789064/107869848-fb693e80-6ed5-11eb-8bb6-8e542855458a.png">

우리가 App에서 Wrapper로 Hello 컴포넌트를 감쌋는데 Hello가 나타나지 않는걸 볼수있다.

이때 우리가 사용할것이 React Children이라는것인데 컴포넌트를 감싸고 안에 어떤것이 표현되야할때 children을 적어주면 된다.

Wrapper를 아래와 같이 수정해보자.

```
import React from "react";

function Wrapper({ children }) {
  const style = {
    padding: "20px",
    border: "2px solid black",
  };
  return <div style={style}>{children}</div>;
}

export default Wrapper;
```

그럼 정상적으로 Hello 컴포넌트들이 보이는것을 알수있다.

<img width="299" alt="스크린샷 2021-02-14 오후 3 06 29" src="https://user-images.githubusercontent.com/56789064/107869863-39666280-6ed6-11eb-96be-c4110fc559dd.png">
