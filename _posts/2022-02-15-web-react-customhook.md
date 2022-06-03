---
layout: post
title:  "Custom hook에 대해서"
subtitle: "Custom hook에 대해서"
categories: web
tags: react
comments: true

---

## Custom hook은 왜 생겼나?

`Custom hook`의 가장 핵심 키워드는 반복되는 로직이라고 할수있다. 즉 반복되는 로직을 묶어서 재사용하기위함 이라고 할수있다.

리액트 공식 홈페이지에는 이렇게 설명되어있다.

```
개발을 하다 보면 가끔 상태 관련 로직을 컴포넌트 간에 재사용하고 싶은 경우가 생깁니다. 
이 문제를 해결하기 위한 전통적인 방법이 두 가지 있었는데, 
higher-order components와 render props가 바로 그것입니다. 
Custom Hook은 이들 둘과는 달리 컴포넌트 트리에 새 컴포넌트를 추가하지 않고도 이것을 가능하게 해줍니다. 
```

`HOC와 render props` 두 경우 모두 컴포넌트가 컴포넌트를 감싸는 형식의 방법이다. 이번에 알아볼 `Custom hook`과는 무슨 차이가있을까?

기존 방법인 두가지 경우는 **컴포넌트를 통해 전달**되는 방식이지만 `Custom hook`은 컴포넌트로 전달되는게 아닌 **해당 컴포넌트**에서 **독립적**으로 `state`를 생성하는 차이다.

**`Custom hook`은 컴포넌트에서 사용을 할때 모두 독립적인 `state`를 가진다.** 이 한 문장을 잘 기억해두자.

## 만약에 Custom hook을 사용하지 않는다면?

`FristComponent` 컴포넌트에서 `API`호출을 통해서 데이터를 받아와 화면에 표현하는 경우를 생각해보자. 

`useEffect`를 통해서 데이터를 받아오고 `useState`를 통해서 컴포넌트 상태를 갱신시키는 아래같은 로직이 될것이다.

```
function FirstComponent() {
  const [data, setData] = useState([]);

  useEffect(() => {

    async function getData() {
      await fetch("http://localhost:3000/api/posts/getAll")
        .then((res) => res.json())
        .then((data) => setData(data));
    }

    getData();
  }, []);

  return (
    <div>
    </div>
  );
}
```

음 여기까지는 좋다. 문제가 없는 코드이다. 그런데 만약에 `FirstComponent`뿐 아니라 다른 `SecondComponent`에서도 똑같은 `API`요청이 있다고 해보자.

```
function SecondComponent() {
  const [data, setData] = useState([]);

  useEffect(() => {

    async function getData() {
      await fetch("http://localhost:3000/api/posts/getAll")
        .then((res) => res.json())
        .then((data) => setData(data));
    }

    getData();
  }, []);

  return (
    <div>
    </div>
  );
}
```

위에서 반복되는 코드가 보이지않는가? 그렇다. `useEffect`와 `useState`가 컴포넌트마다 호출이된다. 그리고 똑같이 `API`호출을 하기도한다.

이런 중복된 과정을 줄이기위해서 **각 상태를 가지는 독립적인 컴포넌트**인 `Custom hook`을 사용해보자.

## Custom hook 사용해보기

`Custom hook`은 이름을 `use`로 시작하는 암묵적 룰이 있다는것을 알아두자.

반복되는 `useEffect`와 `useState`를 하나로 합치기위해서 `useFetch`라는 `Custom hook`을 만들어 보자.

```
function useFetch(url) {

    const [data, setData] = useState([]);
    const [loading, setLoading] = useState(false)

    useEffect(() => {

        async function getData() {
            setLoading(true)
            await fetch(url)
                .then((res) => res.json())
                .then((data) => {
                    setData(data)
                    setLoading(false)
                });
        }

        getData();

    },[])

    return [data,setData,loading]
}
```

이정도 로직이면 괜찮은거같다. 인자로 `Url`을 받아서 `Fetch`를 통해 데이터를 받아오고 

받아온 데이터를 `useState`를 통해 `useFetch`에 **독립적으로**저장한 후에 그 데이터를 돌려주는 로직이다. 또한 `loading`상태도 추가해서 만들수있다. 

이제 우리가 만든 `useFetch` 훅을 `FirstComponent`에서 사용해보자.

```
function FirstComponent() {
  const [data, setData, loading] = useFetch(
    "http://localhost:3000/api/posts/getAll"
  );

  console.log(data, loading);

  return (
    <div>
    </div>
  );
}
```

코드가 너무나도 간단해졌다. `useEffect`와 `useState`를 매 컴포넌트마다 쓸 필요도 없어졋다.

![스크린샷 2022-06-02 오후 8 46 33](https://user-images.githubusercontent.com/56789064/171622491-eed1418b-f011-4c18-b910-28903fc1be05.png)

`console.log`로 출력된 결과가 우리가 원하는대로 나타나는걸 볼수있다. 자 그런데 `Custom hook`은 **독립적**이라고 했다. 선언될떄마다 각기 다른 **상태**를 가진다고도 했다.

시험을 해보기위해 같은 `FirstComponent` 안에서 `url`만 다른 `useFetch`를 써보자.

```
function FirstComponent() {
  const [data, setData, loading] = useFetch(
    "http://localhost:3000/api/posts/getAll"
  );
  const [secondData, setSecondData, secondLoading] = useFetch(
    "http://localhost:3000/api/posts/1"
  );

  console.log("data", data, loading);
  console.log("secondData", secondData, secondLoading);

  return (
    <div>
    </div>
  );
}
```

![스크린샷 2022-06-02 오후 8 59 07](https://user-images.githubusercontent.com/56789064/171624460-d24a7c55-6f89-4d60-a201-9791c053a21e.png)

우리가 원하는 결과를 그대로 받아볼수있다! 만약에 `useFetch()` 훅을 만들지 않았고 `FristComponent`내에서 구현했다면 코드는 아래와 같을것이다.

```
========== Custom hook 사용전


const [data,setData] = useState([])
const [secondData,setSecondData] = useState([])
const [dataLoading, setDataLoading] = useState(false)
const [seconddataLoading, setSecondDataLoading] = useState(false)

useEffect(() => {
    async function getData() {
      setDataLoading(true)
      await fetch("http://localhost:3000/api/posts/getAll")
        .then((res) => res.json())
        .then((data) => {
            setData(data)
            setDataLoading(false)
        });
    }

    async function getSecondData() {
      setSecondDataLoading(true)
      await fetch("http://localhost:3000/api/posts/1")
        .then((res) => res.json())
        .then((data) => {
            setSecondData(data)
            setSecondDataLoading(false)
        });
    }

    getData();
    getSecondData()
},[])

========== Custom hook 사용후

const [data, setData, loading] = useFetch(
    "http://localhost:3000/api/posts/getAll"
);
const [secondData, setSecondData, secondLoading] = useFetch(
    "http://localhost:3000/api/posts/1"
);
```

단순히 눈으로만 봐도 엄청난 코드의 차이를 느낄수있다. 이렇듯 중복되는 코드는 밖으로 빼서 하나의 함수로 만들어서 사용하는것

이것이 `Custom hook`의 사용이유라고 할수있다.

