---
layout: post
title:  "GraphQL 유저요청 데이터를 가져오기"
subtitle: "GraphQL 유저요청 데이터를 가져오기"
categories: antique
tags: antique
comments: true

---

## GraphQL 스키마를 조금더 꼬아보자

스키마의 표현부터 해석해보자.

```
type Query {
  people: [Person]!
  person(id: Int!): Person
}

type Person {
  id: Int!
  name: String!
  age: Int!
  gender: String!
}
```

첫번째 people의 정의를 보자. people을 요청해오면 Person을 넘겨줄것인데 Person은 아래처럼 정의되어 있습니다. 라는 뜻이다.

Person을 감싸고 있는 배열은 여러개가 존재한다는 뜻이다.

두번째 `person(id:Int!) : Person` 이것의 뜻은 무엇일까?

person의 id는 Int 향태이고 id에 맞는 Person의 데이터를 원한다는 뜻이다.

resolver.js 에 우리가 정의한 것을 보자

```
const people = [
  { id: 1, name: "js", age: 27, gender: "female" },
  { id: 2, name: "zz", age: 26, gender: "female" },
  { id: 3, name: "dd", age: 13, gender: "male" },
  { id: 4, name: "ff", age: 12, gender: "male" },
];

const getById = (id) => {
  const filteredId = people.filter((person) => id === person.id);
  return filteredId[0];
};

const resolvers = {
  Query: {
    people: () => people,
    person: (_,args) => getById(args.id),
    // 위와 같은 표현임
    person: (_,{ id }) => getById(id),
  },
};

export default resolvers;
```

일단 People을 먼저 요청해보자.

<img width="803" alt="스크린샷 2021-01-18 오전 12 53 50" src="https://user-images.githubusercontent.com/56789064/104848286-a3b3d380-5927-11eb-82eb-1cbf16bc06f3.png">

요청대로 원하는 데이터가 Person의 형식에 맞게 나오는걸 볼수있다.

자 그럼 다음으로 `person(id:Int)` 이 부분을 처리할것인데.. 

일단 getbyId() 함수를 만들어서 Filter로 같은 아이디만 돌려주도록 만들어주자.

그런데.. 사용자가 우리에게 요청한 id를 어떻게 받을수있을까??

person의 (_,args) 의 표현을 주목해보자. console.log(args)를 설정해두고

playground에서 우리가 person(id:1) 을 요청을 한다면 아래와 같이 결과가 나온다

<img width="537" alt="스크린샷 2021-01-18 오전 1 10 59" src="https://user-images.githubusercontent.com/56789064/104848873-09a15a80-592a-11eb-9119-5bfdeb66b4d4.png">

우린 이 args를 이용해서 resolvers에게 사용자가 요청한 id의 번호를 받을것이고 getbyId 함수로 filter를 하여

<img width="758" alt="스크린샷 2021-01-18 오전 1 16 47" src="https://user-images.githubusercontent.com/56789064/104849003-d7dcc380-592a-11eb-894d-7d176e607b38.png">

id가 1인 데이터만 가져올수있게된다. ES6의 최신문법을 이용하여 { id } 이런방식으로도 쓸수있다.

