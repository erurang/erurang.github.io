---
layout: post
title:  "redux 상태 관리(캐싱) / 라우팅"
subtitle: "redux 상태 관리(캐싱) / 라우팅"
categories: web
tags: redux
comments: true

---

### redux 상태관리(캐싱하기) 

`yarn add react-router-dom` 리액트에서 라우팅을 이용하기위해서 설치해주자.

`pages` 폴더를 만들어 준 후에 `PostListPage.js` `PostPage.js` 두 파일을 만들어주고 아래의 코드를 복붙!

```
// 모든 글 목록을 가져오는 페이지
// PostListPage.js
import React from "react";
import PostListContainer from "../containers/PostListContainer";

const PostListPage = ({}) => {
  return <PostListContainer />;
};

export default PostListPage;

// 특정글을 가져오는 페이지
// PostPage.js
import React from "react";
import PostContainer from "../containers/PostContainer";

const PostPage = ({ match }) => {
  const { id } = match.params;
  const postId = parseInt(id, 10);

  return <PostContainer postId={postId} />;
};

export default PostPage;
```

index.js에서 라우트와 컴포넌트를 추가해주자.

```
// index.js

import { Route } from "react-router-dom";
import PostListPage from "./pages/PostListPage";
import PostPage from "./pages/PostPage";

function App() {
  return (
    <div className="App">
      <Route path="/" component={PostListPage} exact />
      <Route path="/:id" component={PostPage} />
    </div>
  );
}

export default App;
```

![asdfasdf](https://user-images.githubusercontent.com/56789064/118841750-89a0f680-b903-11eb-8279-cc0ba8fcea8a.gif)

잘 작동하는걸 볼수있는데.. 아쉬운점이 있다. `홈화면(/) <--> 특정글 라우터(주소)`를 이동할때마다

매번 홈화면에서 같은 데이터에 대해 로딩을 계속 한다는점이다.. 

`Redux-Logger`를 통한 콘솔로 상태의 흐름을 한번 크게 짚어본뒤에 캐싱을 할수있도록 고쳐보자.

### 상태 흐름을 잡고 캐싱을 해보자

#### PostListContainer (url : /)

![스크린샷 2021-05-20 오전 1 49 22](https://user-images.githubusercontent.com/56789064/118852289-9c202d80-b90d-11eb-8050-4455be013960.png)

홈화면 접속시에 `GET_POSTS` 로 액션이 시작됬음을 알리면서 `loading`을 `false => true`로 바꾼다.

![스크린샷 2021-05-20 오전 1 24 04](https://user-images.githubusercontent.com/56789064/118848821-1353c280-b90a-11eb-9c63-43ea73a2b1dd.png)

API통신 성공으로 `GET_SUCCESS` 액션이 실행된다. `loading`이 `true => false`로 바뀌며 `data`(상태) 가 업데이트된다.

그후에 `<Post>` 컴포넌트가 글목록을 하나하나 만들어준다. 

현재 글은 (3개로 고정되있고) 한번 데이터를 불러온 뒤에는 더 이상의 api통신이 필요하지않다.

그러니까 우리는 `PostContainerList`의 `useEffect`를 아래와 같이 수정해주자.

```
useEffect(() => {
    if (data) return;

    dispatch(getPosts());
  }, [dispatch, data]);
```

처음 스테이트를 받고 난 후에는 리덕스 스토어에 계속 상태가 저장되어있기때문에 매번 `dispatch`를 통해 글목록을 받아올 필요가 없으니 `if(data) return` 을 통해 `useEffect`를 막아주는것이다.

![ㅁㅁㅁ](https://user-images.githubusercontent.com/56789064/118838372-96701b00-b900-11eb-81f0-12acca369889.gif)

이제 한번 데이터를 받은뒤로는 로딩중.. 을 띄우지 않게된다.

글 목록에 대해서는 `data`를 매번 로딩하지않게 만들었다. 

그럼 특정글의 데이터가 있을때도 똑같이 로딩하지않도록 하는 방법은 없을까? 

이번엔 `posts`(특정글)에 대해서 상태를 저장하는 방법을 공부해보자

#### PostContainer (url : /:id)

![스크린샷 2021-05-20 오전 2 07 42](https://user-images.githubusercontent.com/56789064/118854895-2b2e4500-b910-11eb-95c7-184c45cf1fcc.png)

첫번째 글에 들어왔을때의 상태다. `next state`에서 `post:data` 가 딱 하나만 존재하는걸 볼수있다.

그럼 다음으로 두번쨰 글로 가볼까?

![스크린샷 2021-05-20 오전 2 09 31](https://user-images.githubusercontent.com/56789064/118855131-6d578680-b910-11eb-9c95-dc62b5eec613.png)

똑같이 `next state`에서 `post:data` 가 딱 하나만 존재하는걸 볼수있다.. 

음 우리가 아까봣던 `id:1` 의 첫번쨰글은 캐싱이 되고있지 않다는 뜻이기도하다.

이게 상태가 계속 저장되도록 하려면 일단 우리가 처음에 지정한 리듀서부터 고쳐야된다. 

왜냐면 `module/posts.js`로 가서 리듀서를 보면

<img width="445" alt="스크린샷 2021-05-20 오전 2 12 09" src="https://user-images.githubusercontent.com/56789064/118855511-ca533c80-b910-11eb-8fe7-c8d168a55f6f.png">

우리가 매번 `SUCCESS`로 `post`를 성공적으로 받아올때마다 

`post`를 `action.post`로 덮어씌우도록 설정을 했기때문인데..

이걸 해결하려고 `PostListContainer.js`에서 처럼 `useEffect()에 if (data) return;`을 하게된다면

우리가 특정글 하나를 본뒤에 `post.state`에 단 하나의 데이터만 저장이되고, 

`useEffect`는 `data`가 존재하네? 라고 판단하고 더이상의 api호출을 하지않게된다.

그래서 다른 글을 클릭해도 처음 본 글에대한 `state`의 내용만 불러와 지게된다.

그럼 각각의 글에대한 상태저장은 어떤식으로 해야할까?

### 각각 글의 상태를 저장해주기위해 리듀서와 상태를 고쳐주자.

`module/posts.js`에서 기존에 존재하던 `getPostId()` 액션 함수부터 고쳐주자.

```
export const getPostID = (id) => async (dispatch) => {
  // 요청이시작됨
  // dispatch({ type: GET_POST_BY_ID }); 에서 meta : id 라는 값을 하나 더 추가한다.
  // 꼭 키값 이름이 meta가 아니어도됨
  dispatch({ type: GET_POST_BY_ID, meta: id });

  // api호출
  try {
    // 성공
    const post = await postAPI.getPostById(id);

    dispatch({
      type: GET_POST_BY_ID_SUCCESS,
      post,
      // 여기도 meta : id 추가
      meta: id,
    });
  } catch (e) {
    // 실패
    dispatch({
      type: GET_POST_BY_ID_ERROR,
      error: e,
      // 여기도 meta : id 추가
      meta: id,
    });
  }
};
```

액션함수를 고쳐줬으면 그다음엔 리듀서를 저기에 맞게 고쳐줘야한다.

일단 기본상태를 고쳐줘야한다.

```
const InitialState = {
  // 기본상태 설정
  posts: reducerUtils.initial(),
  // post: reducerUtils.initial() => 변경
  post: {},
};
```
왜 빈객체로 설정을하냐면 우리가 하나의 값을 post에 저장해둘것이 아니라, 여러개의 post 값들을 `{ id:{상태값} }`으로 저장을 할것이기 때문에 빈 객체로 초기화시키는것이다.

이젠 리듀서를 고쳐주자.
```
export default function posts(state = InitialState, action) {

    // action.meta에서 id 값을 가져오고
  const id = action.meta;

  switch (action.type) {
    // case GET_POSTS:... 이하생략

    case GET_POST_BY_ID:
      return {
        ...state,
        // post의 상태를 재정의하는데
        post: {
            // 기존의 state.post의 값을 유지하면서
            // 그래야 다른 post의 값이 안날라가기도하고
            // 불변성을 유지함
          ...state.post,
          // 키이름을 [id]로 한채 새로운 스테이트를 지정해주는데

          [id]: reducerUtils.loading(state.post[id]?.data),
          
          // 여기서 왜 loading에 기존데이터가 존재하면 넘겨주는지는 모르겠네.

        },
      };
    case GET_POST_BY_ID_SUCCESS:
      return {
        ...state,
        post: {
          ...state.post,
          [id]: reducerUtils.success(action.post),
        },
      };
    case GET_POST_BY_ID_ERROR:
      return {
        ...state,
        post: {
          ...state.post,
          [id]: reducerUtils.error(action.error),
        },
      };

    default:
      return state;
  }
}
```

다음엔 `postContainer`를 고쳐주자

```
import React, { useEffect } from "react";
import { useSelector, useDispatch } from "react-redux";
import Post from "../components/Post";
import { getPostID } from "../modules/posts";

const PostContainer = ({ postId }) => {
  const { data, loading, error } = useSelector(
    (state) => state.posts.post[postId] || ""
  );

  const dispatch = useDispatch();

  useEffect(() => {
    if (data) return;
    dispatch(getPostID(postId));
  }, [postId, dispatch]);

  if (loading && !data) return <div>로딩중</div>;
  if (error) return <div>에러</div>;
  if (!data) return null;

  return <Post post={data} />;
};

export default PostContainer;
```

여기서 기존과 달라진점은 `state.posts.posts` 를 가져오는게아니라 `state.posts.posts[postId]` 를 data로 가져온다는점이다.

그런데 `state.posts.post[postId]`가 기존 initalState가 {}로 지정되어있어서

`state.posts.post[postId]` 의 `data,loading,error`값을 찾을수가없다.

즉 `undefined`이기때문에 결국 `undefiend.data` 를 조회하는 것이되서 오류가뜨게된다.

그래서 `state.posts.post[postId] || ""` 값이 없다면 빈값이 되게끔 지정해서 오류를 피해준다.

![스크린샷 2021-05-22 오후 8 45 08](https://user-images.githubusercontent.com/56789064/119225405-9a897c00-bb3e-11eb-86db-7bdf64255209.png)

이렇게 하고나면 이제 posts(리듀서이름).post(특정글).id 형식으로 값이 저장되게되는걸 볼수있다.

이제 한번 상태에 저장되고 난 후에는 `useEffect(() => if(data) return ...)` 을 통해서 data가 있으니 같은 정보에 대해서는 로딩이 되지 않는것을 볼수있다.

이렇게 redux thunk에 대해서 기본적인 개념을 모두 끝냈다. 다음에는 redux-saga를 사용하고, redux-thunk와는 무슨 차이인지 공부해보겠다. 