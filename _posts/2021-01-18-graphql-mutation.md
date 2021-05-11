---
layout: post
title:  "GraphQL Mutation"
subtitle: "GraphQL Mutation"
categories: antique
tags: antique
comments: true

---

## GraphQL 추가/삭제/수정

### 추가
데이터를 추가하기위해서 우린 schema에 Mutation 타입을 만들어줘야한다.

아래처럼 만들어주자

```
type Mutation {
  addPerson(name: String!, age: Int!, gender: String!): Person
}
```

addPerson은 name age gender를 인자로 받고 Person을 되돌려준다는 뜻이다.

resolver에는 Mutation을 추가해주자.
```
const addPerson = (name, age, gender) => {
  const newPerson = {
    id: people.length,
    name,
    age,
    gender,
  };
  people.push(newPerson);
  return newPerson;
};

const resolvers = {
  Mutation: {
    addPerson: (_, { name, age, gender }) => addPerson(name, age, gender),
  },
};
```

id를 찾앗을떄와 같이 add로 요청이 들어오면 args에 우리가 요청한 데이터들이 있을것이다.

addPerson의 함수에 인자를 넘겨주어서 기존에 있던 People의 배열에 추가해준다.

return을 통해 만들어진 newPerson을 :Person에 넘겨준다.

playground에서 추가가 되는지 보자.

<img width="822" alt="스크린샷 2021-01-18 오전 2 07 15" src="https://user-images.githubusercontent.com/56789064/104850345-e4b0e580-5931-11eb-990f-1ea37e19b012.png">

Mutation {} 으로 명령어를 지정후 인자들을 넘겨준다. playground에서 Person을 요청하면

<img width="772" alt="스크린샷 2021-01-18 오전 2 07 46" src="https://user-images.githubusercontent.com/56789064/104850362-f85c4c00-5931-11eb-9f4a-3e0a76860502.png">


우리가 새롭게 추가한 person이 업데이트된것을 볼수있다.

### 삭제

```
type Mutation {
  deletePerson(id: Int!): Boolean
}

const resolvers = {
  Mutation: {  
    deletePerson: (_, { id }) => deletePerson(id),
  },
};

const deletePerson = (id) => {
  const needDelete = people.filter((p) => id != p.id);
  if (needDelete) {
    people = needDelete;
    return true;
  } else {
    return false;
  }
};
```

삭제전

<img width="791" alt="스크린샷 2021-01-18 오전 2 30 56" src="https://user-images.githubusercontent.com/56789064/104850913-33ac4a00-5935-11eb-895d-113dcd9c81ab.png">

person의 id가 2인 데이터를 삭제

<img width="821" alt="스크린샷 2021-01-18 오전 2 31 21" src="https://user-images.githubusercontent.com/56789064/104850928-4292fc80-5935-11eb-9a05-d06b0e6446e8.png">

삭제후 

<img width="833" alt="스크린샷 2021-01-18 오전 2 31 46" src="https://user-images.githubusercontent.com/56789064/104850940-52124580-5935-11eb-8f91-bd6770dd8b3d.png">

### 수정

```
type Mutation {
  editPerson(id: Int!, name: String!, age: Int!, gender: String!): Person
}

const resolvers = {
  Mutation: {
    editPerson: (_, { id, name, age, gender }) =>
      editPerson(id, name, age, gender),
  },
};

const editPerson = (id, name, age, gender) => {
  const edit = people.filter((p) => id == p.id);

  if (edit) {
    edit[0].name = name;
    edit[0].age = age;
    edit[0].gender = gender;
    return edit[0];
  }
};
```

수정전

<img width="866" alt="스크린샷 2021-01-18 오전 2 50 54" src="https://user-images.githubusercontent.com/56789064/104851380-fdbc9500-5937-11eb-8f91-dc6e2039d9f8.png">

수정

<img width="891" alt="스크린샷 2021-01-18 오전 2 51 36" src="https://user-images.githubusercontent.com/56789064/104851396-16c54600-5938-11eb-9d49-0c14c676239a.png">

수정후

<img width="891" alt="스크린샷 2021-01-18 오전 2 51 36" src="https://user-images.githubusercontent.com/56789064/104851396-16c54600-5938-11eb-9d49-0c14c676239a.png">

올바르게 수정된 것을 볼수있다.