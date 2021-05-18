---
layout: post
title:  "Redux thunk Promise 사용하기"
subtitle: "Redux thunk Promise 사용하기"
categories: web
tags: redux
comments: true

---

### Promise를 이용해 진짜 api 처리를 공부해보자.

이전 글 `redux thunk 맛보기`에서 객체가 아닌 함수를 디스패치하는것을 볼수있었다.

이번에는 Promise를 이용해서 진짜 api처리를 공부해보자.

### 가짜 API 만들기

`src/api/posts.js` 폴더를 생성해주자.

```
const sleep = (n) => new Promise((res) => setTimeout(res, n));
```

임의로 지정시간 이후에 값을 돌려주는 `Promise`를 만들어주자. 저 함수가 실행되는 순서는 아래와 같다.

```
sleep(1000).then(res => console.log('hello'))
Promise {<pending>} // 펜딩에 들어감....
hello // 1초뒤
```

#### 가짜 글목록 만들기

api를 통해서 받아올 가짜 글목록을 아래처럼 만들어주자.

```
const posts = [
  {
    id: 1,
    title: "아 리덕스",
    body: "진짜 어렵네",
  },
  {
    id: 2,
    title: "그냥 평범한 라이브러린줄 알았더니..",
    body: "대단하게 힘든 라이브러리구나?",
  },
  {
    id: 3,
    title: "내가꼭 이거정복해서",
    body: "제대로 사용하고만다",
  },
];
```

#### 가짜 api 함수

아래함수들은 `Promise(sleep())`를 통해서 일정시간 후에 임의로 설정한 가짜글목록 `posts`를 받아서 리턴해주는 가짜 api라고 가정한 코드다.

```
export const getPosts = async () => {
  await sleep(1000);
  return posts;
};

export const getPostById = async (id) => {
  await sleep(1000);
  return posts.find((post) => post.id === id);
};
```
일단 가짜 `api`를 만들었으니.. 이제 원래 순서인 리덕스로 돌아가자

순서는 늘.. 리덕스의 모듈만들기 -> 컨테이너 고치기 

### 리덕스 모듈만들기

`src/modules/posts.js` 파일 생성해주자.

```
// 임의로 시간을 설정해논 커스텀 api를 가져옴
import * as postAPI from "../api/posts";

// thunk에서 api요청이 일어날때는 필수적으로!!!
// 각 Api마다 액션을 3개씩 만들어야한다.
// 특정 액션을 시작함을 알리는 디스패치 , 성공할시의 디스패치 , 실패함시의 디스패치

// 액션 크리에이터 //
const GET_POSTS = "posts/GET_POSTS";

// api 실패성공
const GET_POSTS_SUCCESS = "posts/GET_POSTS_SUCCESS";
const GET_POSTS_ERROR = "posts/GET_POSTS_ERROR";

// 액션 크리에이터 //
const GET_POST_BY_ID = "posts/GET_POST_BY_ID";
// api 실패성공
const GET_POST_BY_ID_SUCCESS = "posts/GET_POST_BY_ID_SUCCESS";
const GET_POST_BY_ID_ERROR = "posts/GET_POST_BY_ID_ERROR";


// 액션함수

export const getPosts = () => async (dispatch) => {
  // 요청이시작됫음을 알리는 디스패치
  dispatch({ type: GET_POSTS });

  // api호출
  try {
    // 성공했을시에 가짜글목록이 posts로 들어옴
    const posts = await postAPI.getPosts();
    
    // 디스패치로 성공액션을 넘겨주고
    dispatch({
      type: GET_POSTS_SUCCESS,
      // 이건 가짜글목록을 가져오는 api니까..
      posts,
    });
  } catch (e) {
    // 실패
    dispatch({
      // 실패했을시에는 에러 디스패치를
      type: GET_POSTS_ERROR,
      // 에러를 넘김
      error: e,
    });
  }
};

//  특정 글 가져오기
export const getPostID = (id) => async (dispatch) => {
  dispatch({ type: GET_POST_BY_ID });

  try {
    const post = await postAPI.getPostById(id);

    dispatch({
      type: GET_POST_BY_ID_SUCCESS,
      post,
    });
  } catch (e) {
    dispatch({
      type: GET_POST_BY_ID_ERROR,
      error: e,
    });
  }
};

// reducer 생성

const InitialState = {
  // 가짜글목록
  
  posts: {
    loading: false,
    data: null,
    error: null,
  },

  // 특정 글 목록
  post: {
    loading: false,
    data: null,
    error: null,
  },
}

export default function posts(state = InitialState,action) {
  switch (action.type) {

    // 가짜글목록을 가져오는 액션
    case GET_POSTS:
      return {
        // 불변성을 위해
        ...state,

        // posts는 덮어씌움
        posts: {
          // 일단 글목록을 가져오는 pending 상태니까 false -> true로 변경함
          loading: true,
          
          // data와 error는 아직 모르니까 null로 상태지정
          data: null,
          error: null,
        },
      };

    case GET_POSTS_SUCCESS:
      return {

        // 불변성을위해
        ...state,

        // 이게 실행된다는건 GET_POSTS 액션이 실행됫다는거고
        // 성공적으로 데이터를 가져왔다는뜻
        posts: {
          // 데이터를 가져왔으니 false로 변경
          loading: false,

          /*
          위 getPosts() 함수에서 dispatch({ type: GET_POSTS_SUCCESS, posts }); 로 액션함수 안에 posts(가짜글목록)이 전달되었으니.
          data : action.posts 라고 지정하는것임
          */
          data: action.posts,
          // 성공적으로 받앗으니 에러는 null
          error: null,
        },
      };
    case GET_POSTS_ERROR:
      return {
        // 불변성을 위해
        ...state,

        posts: {
          // 실패했으니 로딩은 펄스
          loading: false,
          // 실패했으니 못받아옴 null
          data: null,
          // 액션함수안에 error로 인자를 넘겼으니까 action.error로 지정
          error: action.error,
        },
      };

    // 이하 설명생략. 위와같음
    case GET_POST_BY_ID:
      return {
        ...state,
        post: {
          loading: true,
          data: null,
          error: null,
        },
      };
    case GET_POST_BY_ID_SUCCESS:
      return {
        ...state,
        post: {
          loading: false,
          data: action.posts,
          error: null,
        },
      };
    case GET_POST_BY_ID_ERROR:
      return {
        ...state,
        post: {
          loading: false,
          data: null,
          error: action.error,
        },
      };


    default:
      return state
  }
}
```

하.. 공부하기 힘들다. 그래도 이렇게해보니까 이해가 갔다. 

```
return {
  ...state,
  post : {
    loading: ?,
    data : ?,
    error : ?
  }
}
```

그런데 코드를 보면.. 위 코드가 계속해서 반복되는걸 볼수있다. 

이걸 매 액션마다 다 하나하나 적어준다고? 어우 지옥이 아닐수없다.. 

코드의 노가다성을 줄여주기위해서 임의 객체를 만들어 주는 방법이 있다.

그것은 이 글의 성격과는 조금 멀기때문에 [redux-thunk 리팩토링하기](https://erurang.github.io/web/2021/05/19/web-redux-start9/)에 정리해두었다.

이제 모든 리듀서도 다 만들었다. 이젠 컴포넌트와 컨테이너를 만들자.

아 그전에 `rootReducer`에 posts의 리듀서를 추가해주는것을 잊지말자.

```
import posts from "./posts";

const rootReducer = combineReducers({
  counter,
  todos,
  posts,
});
```

### 프레젠테이션 컴포넌트 만들어주기

`src/components/postList.js`

```
import React from "react";

const PostList = ({ posts }) => (
  <ul>
    {posts.map((post) => (
      <li key={post.id}>
        {post.title},{post.body}
      </li>
    ))}
  </ul>
);

export default PostList;
```

### 컨테이너 컴포넌트 만들어주기

```
import React, { useEffect } from "react";
import { useSelector, useDispatch } from "react-redux";
import PostList from "../components/PostList";
import { getPosts } from "../modules/posts";

function PostListContainer() {
  // useSelector((state) => console.log(state));

  const { data, loading, error } = useSelector((state) => state.posts.posts);

  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(getPosts());
  }, [dispatch]);

  if (loading) return <div>로딩중...</div>;
  if (error) return <div>에러발생!</div>;
  if (!data) return null;

  //   return <p>d</p>;
  return <PostList posts={data} />;
}

export default PostListContainer;
```

여기서 useEffect()를 사용한 이유는 비동기 처리를 위해서 사용한것이다.

![reduxxx](https://user-images.githubusercontent.com/56789064/118720228-552a2d80-b864-11eb-9ce1-ede3fe870ea1.gif)

이렇게 잘 받아오는걸 볼수있다.. 와 정말 내가 언어배울떄보다 더 어려움을 느끼는 라이브러리인거같은 기분이 드는건 왠진 모르겠다..

그런데 이걸 배우는 고생만큼 나에게 엄청 도움이 될거라 생각한다.

다음엔 리덕스로 라우터를 연동하는 방법에대해.. 공부하며 정리하겠다.