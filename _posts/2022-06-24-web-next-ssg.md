---
layout: post
title: "Nextjs SSG란?"
subtitle: "Nextjs SSG란?"
categories: web
tags: nextjs
comments: true
---

## NextJS에서 SSG 사용하기

### SSG는 무엇일까?

`SSG`는 `Static Site Generation`의 약자로 정적사이트 생성이라는 뜻이다. 정적사이트생성이란 `HTML`을 만들어둔다는 것이다.

`HTML`을 만든다? 감이 오지 않을수있다. 실습을 해보며 익혀보자

## Pages/staticprops.js 생성

연습을위해 `Pages`폴더안에 `staticprops.js`를 만들어주자.

```
import { useState, useEffect } from "react";

export default function PracticeStaticProps() {
  const [data, setData] = useState();

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/todos/1")
      .then((response) => {
        if (!response.ok) throw new Error("응답실패");

        return response.json();
      })
      .then((json) => setData(json))
      .catch((e) => console.log(e));
  }, []);

  return (
    <div className="App">
      <h1>staticprops page</h1>
      <h1>{data?.title}</h1>
    </div>
  );
}
```

위의 코드는 `Nextjs`가 `CSR`로 동작하는 코드이다. `API`에서 데이터를 받아와 `title`을 표현하는 코드이다.

`npm run build`를 한후에 `.next/server/pages`폴더 안을 보면 `pre-generate`된 `HTML`파일들 중에

<img width="477" alt="스크린샷 2022-06-24 오전 8 51 33" src="https://user-images.githubusercontent.com/56789064/175433265-3767d18c-d327-481e-b78a-2ca976db29ff.png">

우리가 만든 `staticprops.html`을 열어보면 `data.title`이 존재하지않은 상태로 만들어진걸 볼수있다.

`SSG`를 사용하는 이유는 여기서나온다. `Client`에서 데이터를 불러와서 `Browser`에서 `CSR`을 하는게 아닌

`SSG`를 사용해서 빌드시에 아예 데이터가 존재하는 `HTML`로 만들어버리는것이다.

`SSG`하기 위해선 `getStaticProps()` 라는 함수를 이용하게된다. 코드를 아래처럼 수정해주자

```
export default function PracticeStaticProps({ data }) {
  return (
    <div className="App">
      <h1>staticprops page</h1>
      <h1>{data.title}</h1>
    </div>
  );
}

export async function getStaticProps() {
  const data = await fetch("https://jsonplaceholder.typicode.com/todos/1").then(
    (response) => {
      if (!response.ok) throw new Error("응답실패");

      return response.json();
    }
  ).catch(e => console.log(e));

  return {
    props: {
      data,
    },
  };
}
```

우리는 더이상 `PracticeStaticProps` 컴포넌트에서 `useEffect`나 `useState`를 통해 상태를 관리하고 `API`를 불러올 이유가 없다.

`getStaticProps()`에서 데이터를 불러와서 `PracticeStaticProps` 컴포넌트에 주입을 해주게된다.

주입을 해주기위해서 우리는 `props:{}`라는 이름으로 넘겨주고자하는 데이터를 `key:value`형식으로 `return` 해주면 된다.

그러면 `PracticeStaticProps` 컴포넌트에서 `props`를 통해 값을 받게되고. 데이터가 HTML로 바로 만들어지는것이다.

`HTML`에 데이터가 잘 만들어졋는지 테스트해볼까? 이전과 같이 `npm run build`를 한 후에

<img width="527" alt="스크린샷 2022-06-24 오전 9 02 49" src="https://user-images.githubusercontent.com/56789064/175434626-b9c0935e-7a1e-45b2-8d6a-265a6e4fbe8c.png">

`.next/server/pages`폴더 안 `staticprops.html` 파일을 보자.

`CSR`에서는 `<h1>staticprops page</h1>`만 있엇다면

`SSG`에서는 `<h1>staticprops page</h1> <h1>delectus aut autem</h1>`로

`API`로 받아온 데이터를 바로 `HTML`에 적용되어 `HTML`자체가 만들어진것을 볼수있다.

즉 `SSG`를 통해서 미리 데이터를 다 받은 미리 만들어진 `HTML`을 사용자에게 주므로 사용자 경험에 이점이 있고

불필요한 요청을 서버에 하지않아도 된다는것을 알수있다.