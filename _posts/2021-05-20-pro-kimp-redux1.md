---
layout: post
title:  "리덕스 입혀보기"
subtitle: "리덕스 입혀보기"
categories: project
tags: kimp
comments: true

---

만들어진 기준의 날짜의 [깃허브 ReadME.md](https://github.com/erurang/btckimp/tree/1886cfed7165fdc295619598fb49630639e3976b)

지금 만들어진 프로젝트에선 각각의 컴포넌트가 하나씩 상태를 가지고있으면서 상태변화를 감지하고 바꾸는 상태였는데, 

이 컴포넌트 상태를 한곳에서 관리해서 하단 컴포넌트에 직접적으로 쏴주고싶은 생각이 들었다.

리덕스의 필요성을 절실히 느꼇고, 일단 간단한 리덕스 적용부터 시작하려한다.

`create-react-app`의 기존 상태에서 시작한다.

```
yarn add redux
yarn add react-redux
yarn add redux-thunk
```

src 폴더내에 리덕스 연습용도로 한개의 코인부터 state로부터 불러오는 연습을 해본다.

### 리듀서와 액션생성자 만들기

#### coin.js
```
import axios from "axios";

const GET_COIN = "coin/GETCOIN";

const GET_COIN_SUCCESS = "coin/GET_COIN_SUCCESS";
const GET_COIN_ERROR = "coin/GET_COIN_ERROR";

export const getCoin = () => async (dispatch) => {
  dispatch({ type: GET_COIN });

  try {
    const payloadRes = await axios.get(
      "https://api.upbit.com/v1/ticker?markets=KRW-BTC"
    );
    const payload = payloadRes.data[0];

    dispatch({
      type: GET_COIN_SUCCESS,
      payload,
    });
  } catch (e) {
    dispatch({
      type: GET_COIN_ERROR,
      error: e,
    });
  }
};

const initialState = {};

export default function coin(state = initialState, action) {
  switch (action.type) {
    case GET_COIN:
      return {
        ...state,
        payload: { loading: true, data: null, error: null },
      };

    case GET_COIN_SUCCESS:
      return {
        ...state,
        payload: { loading: false, data: action.payload, error: null },
      };

    case GET_COIN_ERROR:
      return {
        ...state,
        payload: { loading: false, data: null, error: action.error },
      };
    default:
      return state;
  }
}
```

#### reducer.js
```
import { combineReducers } from "redux";
import coin from "./coin";

const rootReducer = combineReducers({
  coin,
});

export default rootReducer;
```

#### index.js
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { Provider } from "react-redux";
import rootReducer from "./reducer";
import { createStore, applyMiddleware } from "redux";
import logger from "redux-logger";
import { composeWithDevTools } from "redux-devtools-extension";
import ReduxThunk from "redux-thunk";

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(ReduxThunk, logger))
);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```
#### CoinListContainer.js

```
import React, { useEffect } from "react";
import { useSelector, useDispatch } from "react-redux";
import { getCoin } from "../modules/coin";
import CoinListComponent from "./CoinListComponent";

const CoinListContainer = () => {
  const { loading, data, error } = useSelector(
    (state) => state.coin.payload || ""
  );

  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(getCoin());
  }, [dispatch]);

  if (loading) return <div>로딩중...</div>;
  if (error) return <div>에러발생!</div>;
  if (!data) return null;

  return <CoinListComponent data={data} />;
};

export default CoinListContainer;
```

#### CoinListComponents.js

```
import React from "react";

const CoinListComponent = ({ data }) => {
  return (
    <div>
      <p>{data.market}</p>
      <p>{data.trade_price}</p>
    </div>
  );
};

export default CoinListComponent;
```

#### App.js

```
import CoinListContainer from "./CoinListContainer";

function App() {
  return (
    <div className="App">
      <CoinListContainer />
    </div>
  );
}

export default App;
```

이렇게 파일을 설정해준뒤에 내 react app을 실행해주면 우리가 원하는대로 콘솔을보면

![스크린샷 2021-05-20 오후 11 17 09](https://user-images.githubusercontent.com/56789064/118994700-82402280-b9c1-11eb-9804-101b757fcbda.png)

올바르게 코인의 정보를 스토어가 가지고있고 스토어의 상태정보로 

![스크린샷 2021-05-20 오후 11 20 31](https://user-images.githubusercontent.com/56789064/118995277-fa0e4d00-b9c1-11eb-8bfe-a2bc8e299113.png)

컨테이너 -> 컴포넌트 형태로 올바르게 내 정보가 올라온걸 볼수있다.

자 다음할일. 코인의 전체 리스트를 표현하고 코인을 누르면 그 코인의 가격정보를 표시하는것

