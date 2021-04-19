---
layout: post
title:  "Webpack dev server"
subtitle: "Webpack dev server"
categories: web
tags: webpack
comments: true

---

## 웹팩 데브서버

웹팩 데브서버는 빌드대상 파일이 내용이 변경되었을때 자동으로 웹팩이 적용하여 브라우저를 새로고침해주는 도구입니다.

```
"scripts": {
  "dev": "webpack serve",
  "build": "webpack"
}
```
빌드가 되어서 파일에 적용된것은 아니고 빌드전에 미리 내가 수정한 코드가 어떻게 작동하는지 알수있는 방법입니다. (라이브서버라고 보면될듯)

그래서 웹팩 데브 서버는 개발할 때만 사용하다가 개발이 완료되면 웹팩 명령어를 이용해 결과물을 파일로(빌드해서) 생성해야 합니다.

```
// webpack.config.js
module.exports = {
  devServer: {
    proxy: {
      '/api': 'http://localhost:3000',

      <!-- changeOrigin: true -->
      <!-- 만약에 도메인이름이 ip주소가 아닐 다르다면 위옵션 추가 -->
    }
  }
};
```

### HMR

브라우저를 새로 고치지 않아도 웹팩으로 빌드한 결과물이 웹 애플리케이션에 실시간으로 반영될 수 있게 도와주는 설정입니다. 
브라우저 새로 고침을 위한 LiveReload 대신에 사용할 수 있으며 웹팩 데브 서버와 함께 사용할 수도 있습니다.

```
module.exports = {
  devServer: {
    hot: true
  }
}
```