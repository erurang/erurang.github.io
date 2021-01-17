---
layout: post
title:  "GraphQL을 쓰는 이유"
subtitle: "GraphQL을 쓰는 이유"
categories: db
tags: graphql
comments: true

---

## graphQL을 쓰는 이유?

REST API는 필요한 데이터만 가져오기 위해선 원하는 양식으로 여러번을 요청해야합니다.

내가 사용하지않는 데이터까지 받아오게 됩니다.

내가 필요하지않는 데이터까지 받아오는것은 효율적이지 못합니다.

이것을 `Over-fetching`이라고 합니다

특정 데이터를 정해서 받아오려할때

```
/feed/comments
/feed/likeNumber
/noti/
/user/username
/user/profile
```

이렇게 특정 데이터를 얻기위해 REST API는 5번의 요청을 하여야 합니다.

이것을 `Under-fetching` 이라 합니다.

### GraphQL 에서는 5번의 요청이 아닌 한번의 요청으로 원하는 정보만 받을수 있습니다.

저것을 어떻게 한번에 받을수 있을까요? 단 한번 graphql서버에 Post요청으로 원하는 데이터만 받을수 있습니다.

## 설치

> yarn add graphql-yoga

기본적인 필수 설정만 들어가있습니다.

