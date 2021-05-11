---
layout: post
title:  "GraphQL 서버만들기"
subtitle: "GraphQL 서버만들기"
categories: antique
tags: antique
comments: true

---

## GraphQL 서버만들기

앞서 설치과정에 따라 `yarn add graphql-yoga` 가 설치되어있을것이다.

우리의 index.js 파일에서 서버를 만들어보자.

![image](https://user-images.githubusercontent.com/56789064/104839984-01382800-5908-11eb-9a9a-f29f00d45b6e.png)

서버를 단순히 시작만 한 코드다. 그리고 콘솔을 보면

<img width="622" alt="스크린샷 2021-01-17 오후 9 08 44" src="https://user-images.githubusercontent.com/56789064/104840021-317fc680-5908-11eb-8fee-d81072efecc0.png">

`No schema defined` 오류를 볼수있다. 스키마는 일종의 데이터의 형식인데

사용자에게 보내거나 사용자에게 받을 데이터에 대한 형식이라고 보면 된다.

단순히 우리는 GraphQL 서버를 켯을뿐 스키마가 없어서 오류가 뜬것이다.

## Schema

`GraphQL`에서의 명령은 2가지가 있다.

첫째로 `Query` 는 데이터를 요청해서 정보를 받을때 사용하고

둘쨰로 `Mutation` 는 추가/수정/삭제를 해주는 명령어다.

즉 우리는 `GraphQL`에 어떤 `Query`와 `Mutation`을 가지고있는지 알려주어야한다.

그래서 스키마를 만든다. 현재 폴더내에 `graphql/schema.graphql` 을 만들어보자.

그리고 `schema.graphql`에 `query`와 mutation 들을 정의해주자

```
type Query {
  name: String!
}
```

이 파일은 단지 어떤 사용자가 `Query`에 `name`을 요청하면 `String`을 돌려준다는 설명만 했을뿐이지

다른 데이터적인 로직이 처리된것이 아니다. 이제 우리가 할것은 2가지.

`graphQL`에게 `schema`의 형식을 알려주고 형식에 대한 기능을 만들어줘야한다.

## Resolver
기능은 `graphql/reslover.js` 라는 파일을 새로만들어 정의하도록하자.

```
const resolvers = {
  Query: {
    name: () => "erurang",
  },
};

export default resolvers;
```

누군가 `Query`로 `name`을 요청하면 `erurang`을 넘겨준다는 뜻이다.

이제 이 `schema`와`resolver`를 서버에 만들어졌다고 알려야한다.

`index.js`에 아래와 같이 추가해주자

```
import { GraphQLServer } from "graphql-yoga";
import resolvers from "./graphql/resolvers";

const server = new GraphQLServer({
  // 모든 타입들에 대한 정의
  typeDefs: "graphql/schema.graphql",
  // 타입이 하는 기능
  resolvers,
});

server.start(() =>
  console.log(`Graphql Server Running http://localhost:4000`)
);
```

<img width="347" alt="스크린샷 2021-01-18 오전 12 10 28" src="https://user-images.githubusercontent.com/56789064/104847129-94ca2280-5921-11eb-88a1-aeb8067186db.png">

서버가 정상 작동되는걸 볼수있다. 사이트로 들어가면 graphql playground를 볼수있다.

아래는 `Query`로`name`을 요청한 결과로 우리가 위에서 설정한 것과 똑같은 것을 볼 수 있다.
<img width="1025" alt="스크린샷 2021-01-18 오전 12 11 57" src="https://user-images.githubusercontent.com/56789064/104847155-c93dde80-5921-11eb-9096-9ed49145c86c.png">