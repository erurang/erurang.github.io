---
layout: post
title:  "REST API를 GraphQL로 가져오기"
subtitle: "REST API를 GraphQL로 가져오기"
categories: antique
tags: antique
comments: true

---

## REST API를 GraphQL로 가져오기

우리가 graphQL을 배우는 이유가 무엇인가!

REST API 의 무분별한 데이터를 받고싶지 않아서가 아닌가!!..

그래서 어떤 API를 사용해서 실습을 해볼까 하다가 네이버영화검색API를 이용했다.

포스트맨에서 시도해보니 아주 잘되는걸 볼수있다. 
<img width="927" alt="스크린샷 2021-01-18 오전 6 14 35" src="https://user-images.githubusercontent.com/56789064/104856187-703b6e00-5954-11eb-9df6-e7ce1d9db0ba.png">


일단 스키마부터 정의해야한다. movies에는 여러개의 movie들이 들어있고 

네이버API에 요청후 날아온 정보들 중 필요한것들의 타입을 지정해주었다.

### schema.graphql
```
type movie {
  title: String!
  subtitle: String!
  link: String!
  image: String!
  pubDate: String!
  userRating: String!
  actor: String!
  director: String
}

type Query {
  movies: [movie]!
}
```

### db.js

다음으로 db파일을 따로만들어서 axios를 이용해 데이터를 처리했다.

만약에 return을 해주지 않으면 `cannot return null for non-nullable field..` 

오류가 뜨게되니 return은 꼭 넣어주자.

네이버API 검색어 사용조건이 "검색어(UTF-8인코딩 필요)" 였기떄문에 한글을 인코딩 해줘야했다.

`encodeURI() 함수는 URI의 특정한 문자를 UTF-8로 인코딩해 하나, 둘, 셋, 혹은 네 개의 연속된 이스케이프 문자로 나타냅니다.`


```
import axios from "axios";

const test = encodeURI("아이언맨");

const config = {
  method: "get",
  url: `https://openapi.naver.com/v1/search/movie.json?query=${test}`,
  headers: {
    "X-Naver-Client-Id": "######",
    "X-Naver-Client-Secret": "######",
  },
};

const movie = axios(config)
  .then(function (response) {
    return response.data.items;
  })
  .catch(function (error) {
    console.log(error);
  });

export default movie;

```

### resolvers.js

그리고 resolvers에 movies에는 movie를 받아올것이라고 정의해두었다.
```
import movie from "./db";

const resolvers = {
  Query: {
    movies: () => movie,
  },
};

export default resolvers;

```

### 결과

REST API를 GraphQL로 적용시킨후 내가 원하는 정보들만 요청하여 받아올수있게 되었다

<img width="963" alt="스크린샷 2021-01-18 오전 6 22 05" src="https://user-images.githubusercontent.com/56789064/104856343-7f6eeb80-5955-11eb-990d-4a4c95a2b75b.png">

### graphQL은 형식일뿐!!

프론트엔드나 백엔드에서 graphQL에 정보를 요청하고 답을 해주는 서비스가 필요한데..

Apollo는 프론트와 백을 둘다 지원하는 아주 좋은 라이브러리이다.