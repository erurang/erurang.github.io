---
layout: post
title: "Nextjs useSWR 사용해보기"
subtitle: "Nextjs useSWR 사용해보기"
categories: web
tags: nextjs
comments: true
---

## useSWR 사용해보기

리액트에서 react-query로 데이터를 캐싱하고 불러오기 기능을 했었다.

넥스트에서는 swr 라이브러리를 제공하는데, 이 라이브러리도 리액트쿼리와 같이 key:value의 값으로 API데이터를 캐싱하고 불러오게된다.

넥스트 공식문서에 사용경우를 이렇게 정의해두었다.

```
CSR 데이터 요청은 SEO를 신경쓰지 않을떄 유용하다. 즉 데이터를 pre-render할 필요가 없거나 데이터가 자주 페이지에 업데이트될때를 뜻한다.
하지만 불행하게도 SSR은 CSR을 사용할수가 없다.

```

asfdafsd
