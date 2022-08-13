---
layout: post
title: "객체의 깊은 복사방법"
subtitle: "객체의 깊은 복사방법"
categories: web
tags: javascript
comments: true
---

## 객체의 깊은 복사방법에 대해 알아보자

나는 이전글에서 [객체복사 방법](https://erurang.github.io/web/2022/03/02/js-object/)에 대해서 글을 적은적이있다.

이 글에서 한가지 더 나아가 객체의 깊은 복사에대해 알아보고자한다.

이 문제점을 나는 회사에서 프로젝트를 하면서 겪게되었다.

문제의 발단은 이렇다. 나는 서버에서 데이터를 받아와 객체상태를 갱신하는 방법이였다.

**프로젝트와 형식이 같지않음을 미리 밝혀둔다!!.**

```
const [data, setData] = useState({})
```

상태 갱신을 위한 `useState`를 만들었다.

```
{
  id: 0,
  name: "name",
  value: 0,
  email : ''
}
```

서버에서 이런 형태로 데이터를 나에게 넘겨주면 나는 위 데이터를

```
{
    [id] : {
        [name] : {
            value,
            email
        }
    }
}
```

이렇게 고친다. 그 다음에 UI를 아래처럼 만들어낸다 여기서 `id`는 현재 보고있는 화면 숫자라고 생각하면 된다.

```
return (
    ...
    {data && data[id] && data[id].map(data => <span>{data.name?.value}</span>)}
    ...
)
```

## 자 다음으론 내가 왜 이 글을 쓰게 됬는지에 대한 이유가 나온다.

만약에 서버에서 요청을 2번 하여 응답을 2번 받았다고 해보자. 그 데이터를 가공하여 위 형식대로 만들었다.

```
const response1 = {
    0 : {              // id
        "1" : {        // name
            value: 1,
            email : '1'
        }
    }
}

const response2 = {
    0 : {              // id
        "2" : {        // name
            value: 2,
            email : '2'
        }
    }
}
```

다음으론 이 데이터들를 `state`에 갱신하기위헤 `setData`를 하였다.

```
setData({...response1, ...response2})
```

`spread operator`로 객체를 합쳤다. 당연히 우리가 예상한 결과는 아래와 같을것이다.

```
{
    0 : {
        "1" : {
            value: 1,
            email : '1'
        },
        "2" : {
            value: 2,
            email : '2'
        }
    }
}
```

`0`번째 `key`인 `1` 과 `0`번째 `key`인 `2`가 중복되지않으니 2개의 키가 온전히 합쳐질것이라 생각하엿다.

하지만 결과는?

```
{
    0 : {
        "2" : {
            value: 2,
            email : '2'
        }
    }
}
```

어 우리가 에상한 결과와 다르다. 많이다르다. 우리의 `key`값 `1`의 `key:value`는 어디로 간것인가?

여기서 우리가 알수있는것이있다. `spread operator`와 `Object.assign` 이 두가지방법은

객체를 합칠때 얕은 복사를 이용한다는 것이다.

## 객체의 깊은복사.

객체의 깊은 복사 방법에 가장 유명한 라이브러리 [deepmerge](https://www.npmjs.com/package/deepmerge)가 있다.

![스크린샷 2022-08-13 오후 5 38 33](https://user-images.githubusercontent.com/56789064/184476143-eb61f7c5-96c5-40af-8f46-7d7aaf235f1e.png)

주간 다운로드횟수가 2천만일 정도로 아주 유명한 라이브러리이다. 이 라이브 러리를 통해서 위 객체를 합쳐보자.

```
import merge from "deepmerge"

const response1 = {
    0 : {              // id
        "1" : {        // name
            value: 1,
            email : '1'
        }
    }
}

const response2 = {
    0 : {              // id
        "2" : {        // name
            value: 2,
            email : '2'
        }
    }
}

merge(response1,response2)
```

결과는?

```
{
    0 : {
        "1" : {
            value: 1,
            email : '1'
        },
        "2" : {
            value: 2,
            email : '2'
        }
    }
}
```

정말 우리가 원하던 결과가 나왔다.

우리가 평소에 이용하던 `spread Operator`나 `Object.assign`은 객체를 얕게 복사한다는것을 명심하자.
