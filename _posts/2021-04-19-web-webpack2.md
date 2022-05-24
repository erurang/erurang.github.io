---
layout: post
title:  "Webpack config.js Setup"
subtitle: "Webpack config.js Setup"
categories: web
tags: webpack
comments: true

---

## webpack.config.js 

프로젝트의 루트 폴더에 `webpack.config.js` 파일을 만들어준다.

웹팩은 최신코드(ES6...)를 이해하지 못한다. 그렇기때문에 옛날 자바스크립트문법을 사용해야 한다.

```
module.exports = {}
```

### index.js

루트폴더에 `index.js` 생성

```
async function hello() {
  await fetch("")
  console.log('hello')
}

hello()
```

이 최신 자바스크립트 문법이 어떻게 변화될지 후에 지켜보자.

### package.json

```
"scripts": {
    "build": "webpack",
},
```

프로젝트의 루트 폴더의 `package.json script` 명령어를 위처럼 수정해준다. `webpack`이 `webpack.config.js`를 보고 번들링을 해주게된다.

웹팩의 module 속성은 총 `entry output loader plugin` 4가지가 있다. 하나하나 알아보자.

## entry output mode

### webpack.config.js

```
module.exports = {
  entry : "최초 시작점 자바스크립트 경로"
  output : {
    filename : '웹팩이 번들링을 끝낸후 만들 파일 이름',
    path : path.resolve(__dirname, '번들링한 파일이 저장될 경로')
  }
}
```

`entry `속성에는 웹팩이 번들링을 시작할 자바스크립트 파일을 정해준다. 

`output`에는 웹팩이 번들링을 끝낸후 파일이 저장될 지점과 이름을 정해준다.

`entry`를 여러개(하나의 js파일이 아닌 별도의 js파일로 번들링)를 두고싶다면 
```
entry : {
        main : './src/js/main.js',
        router : './src/js/router.js'
},
output: {
        filename : "js/[name].js",
        path : path.resolve(__dirname, "build"),
},
```
`entry`를 `output파일명 : '경로'`로 설정해둔후 `output`의 `filename`을 `[name].js`로 수정하면

`entry`에 설정한 파일명으로 `output js`가 만들어지게된다. 만약에 `[name]` 동적할당을 사용하지않으면 

![스크린샷 2022-05-24 오후 9 37 38](https://user-images.githubusercontent.com/56789064/170036374-d7e2c4c8-877e-4854-9471-68c70666bf1c.png)

`main.js`로 만들어질 파일이 2개여서 경고문이 나오게된다.


### 모듈링 해보기

```
module.exports = {
  entry : "./index.js"
  output : {
    filename : 'main.js',
    path : path.resolve(__dirname, 'assets', 'js')
  }
}
```

만약에 위처럼 설정을 후에 `npm run build` 를 해보자.

![스크린샷 2022-05-24 오후 7 48 08](https://user-images.githubusercontent.com/56789064/170017755-b7d49c43-b9ec-41b8-a4d1-a7131df5b4cb.png)

웹팩이 `js`파일을 모듈링 해서 하나의 파일로 주었고

![스크린샷 2022-05-24 오후 7 54 48](https://user-images.githubusercontent.com/56789064/170018902-8e727211-67b7-47f5-a6a6-19b85ce8634c.png)

되게 이상한(?) 코드로 웹팩이 변경시켰다. 이 코드가 브라우저가 이해하는 아~~주 못생긴 코드라고 할수있다. 

하지만 `npm run build` 명령어를 쳤을때 아래 경고문이 뜨며 `webpack.config.js`에서 `mode`를 설정해달라고 한다.

![스크린샷 2022-05-24 오후 7 47 21](https://user-images.githubusercontent.com/56789064/170017638-6e569b17-9543-4c34-83cd-3cf4269375d4.png)

`mode`에는 `development production` 2가지의 `mode`가 있는데 설정을 해주지 않으면 기본적으로 `production`(배포) `mode`가 기본으로 설정된다.

`production` 배포모드는 `Webpack` 모듈 번들링 과정에서 자체적으로 코드를 최적화하여 용량을 줄여준다. 즉 웹페이지의 속도 향상을 얻어올수 있는것이다.

`development` 개발모드는 `React`로 예를들어보자면, `React`의 개발 모드는 버그로 이어질만한 많은 부분을 미리 경고해주는 검증 코드가 포함되어 있다.

하지만, 이런 코드는 사용하는 사용자 입장에서는 필요가 없는 부분이다. 즉 번들 크기를 증가시키고 웹페이지 속도를 느리게 할 수 있다. 

![스크린샷 2022-05-24 오후 8 02 12](https://user-images.githubusercontent.com/56789064/170020157-a937d242-87c7-4309-8c3e-6589df35b49f.png)

배포단계에서는 `webpack`의 `production` 배포모드를 이용하여 `js`파일의 크기를 줄이는것이 중요하다고 할수있다.


## loader

`loader`에 대해 알아보기 위해 루트폴더에 `test.css`를 하나 만들어준후 아래처럼 적어주자

```
body {
    background-color: red;
}
```

그리고 루트에 있는 `index.js`에 `import`로 자바스크립트에서 `css`파일을 불러오자

```
import "./test.css"
...
```

자 이전처럼 `js`파일을 `npm run build` 명령어를 통해 번들링을 해보자.


![스크린샷 2022-05-24 오후 9 02 23](https://user-images.githubusercontent.com/56789064/170030040-cb5302a0-6be6-4b01-a12d-d50b5b0ff4e4.png)

오류가 우리를 반기는데 `webpack`이 이 파일을 핸들링하는 방법을 모른다는 뜻이다.

즉 `loader` 속성은 `webpack`이 어떤 특정 파일을 봣을때 `webpack`이 어떻게 해석할지를 알려주는것이다.

```
npm i css-loader style-loader -D
```

`css-loader`는 `webpack`이 `import url()`구문을 해석하는 방법을 알려주는것이고

`style-loader`는 `webpack`이 `DOM`에 `css`를 입히는 방법을 알려주는 `loader`이다

### webpack.config.js

```
module.exports = {
  module: {
    rules: [
      { 
        test : 로더를 적용할 파일 (일반적으로 정규표현식 사용),
        use : [ test파일에 적용할 로더의 이름 ] or "test파일에 적용할 로더의 이름"
      }
    ]
  }
}
```

`rules`에 `loader`의 세부사항을 설정해준다. 우리는 `webpack`이 `css`파일을 해석하기를 원하기때문에 아래처럼 적어줄수있다.

```
module.exports = {
  module: {
    rules: [
      { 
        test : test: /\.css$/,
        use : ['style-loader','css-loader']
      }
    ]
  }
}
```

`.css` 확장자로 끝나는 모든파일은 `css-loader -> style-loader` 순서로 웹팩이 해석해야 한다 라는 뜻이다.

`webpack`이 `loader`를 해석할때는 오른쪽에서 왼쪽 순서대로 해석한다는것에 주의하자.

![스크린샷 2022-05-24 오후 9 21 35](https://user-images.githubusercontent.com/56789064/170033483-f32ec71b-b6a0-424a-bbd5-6feb043f1abd.png)


`webpack`이 이제는 파일을 해석하는 방법을 알기때문에 `npm run build`를 통해 성공적으로 번들링한것을 볼수있다.

하지만 위처럼 번들링을 하게되면 `js`파일에 `css`파일이 얹어져서 `js`파일의 용량이 올라가게되어 속도에 영향을 미치게 된다.

즉 `js`는 `js`끼리 `css`파일을 `css`끼리 따로 번들링시키면 좋은데 이런 추가적인 기능은 `plugin` 속성을 이용할수있다.

만약에 우리 프로젝트의 `css`가 `sass`를 사용하고 `js`의 문법을 최신으로 사용하고싶다면?

```
module.exports = {
  module: {
    rules: [
      { 
        test : test: /\.scss$/,
        use : ['style-loader','css-loader', 'sass-loader']
      },
      {
        test : /\.js$/,
        use : {
              loader : 'babel-loader',
              options : {
                  presets: [['@babel/preset-env', { targets: "defaults" }]]
              }
        }
      },
    ]
  }
}
```

## plugin

플러그인은 `webpack`이 `output`으로 만든 파일의 결과물을 최종적으로 변경한다.

즉 `loader`가 파일을 해석하고 변환하는 과정이라면 `plugin`은 결과물의 형태를 바꾸는 역할을 한다.

```
npm i mini-css-extract-plugin -D
npm i html-webpack-plugin -D
```

`mini-css-extract-plugin`는 모듈이 번들링될때 `css`와 `js`파일이 합쳐지지않고 개별로 번들링 되도록 해주는 플러그인,

`html-webpack-plugin`는 `html`을 템플릿으로 생성할 수 있게 도와주는 플러그인인데 `filename`으로 `html`파일을 지정해줄수있다.

### 사용하기

```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin")

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      filename : '시작점이 되는 html경로 지정'
    }),
    new MiniCssExtractPlugin({
      filename : 'output이될 css파일의 경로'
    })
  ],
  rules: [
      { 
        test : test: /\.css$/,
        use : [ MiniCssExtractPlugin.loader,'css-loader' ]
      },
    ]

}
```

## webpack-dev-server

번외로 `live-reloading`을 해주는 `dev-server`를 설정할수있다.

`webpack-dev-server`는 빌드대상 파일이 내용이 변경되었을때 자동으로 `webpack`이 적용하여 브라우저를 새로고침해준다.

즉 빌드가 되어서 파일에 적용된것은 아니고 빌드전에 미리 내가 수정한 코드가 어떻게 작동하는지 알수있는 방법이다.

`vscode`의 경우에 `Liveserver extension`을 사용할수도 있지만 `webpack`에서 설정하는 법도 알아보자.

### 설치
```
npm i webpack-dev-server -D
```

### pacakge.json script 수정
```
"dev": "webpack serve"
```

### webpack.config.js 설정

```
module.exports = {
  ...,
   devServer : {
        open : true, // devserver 구동후 브라우저열기
        compress: true, // 파일을 압축함
        hot : true, // 웹에 실시간으로 반영될 수 있게 도와줌 
        port : 5500, // 오픈할 포트
        proxy : { 
            // cors를 방지하기위한 api주소
            '/api' : 'http://localhost:3000'
        }
    }
}
```

정말 기본적으로 사용되는 부분만 전체적으로 정리하였다. 이전에도 말했지만 대부분의 경우에

자체적으로 `Webpack.config.js`가 설정이 되어있기때문에 뜯어볼 경우가 크게 없겠지만

웹팩이 생겨난 이유, 웹팩의 기본적 작동방식을 익히기에는 충분할것이라고 생각한다.
