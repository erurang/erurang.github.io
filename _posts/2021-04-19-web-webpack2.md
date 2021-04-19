---
layout: post
title:  "Webpack의 사용이유와 기본셋팅"
subtitle: "Webpack의 사용이유와 기본셋팅"
categories: web
tags: webpack
comments: true

---

## 웹팩의 주요속성

webpack.config.js에서 쓰이는 모듈의 속성들에는 4가지가 있다.

```
entry
output
loader
plugin
```

### entry

```
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

entry 속성은 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점이자 자바스크립트 파일 경로입니다.

entry에서 지정한 속성을 통해서 웹팩이 모든 의존성을 파악해 파일을 만들기때문에 가장 출발점이 되는 파일을 지정해야한다.

### output

```
var path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

output 속성은 entry 웹팩을 통해 만들어진 결과물의 파일이 저장됩니다.

bundle.js라는 이름으로 ./dist의 폴더에 생성된다는 뜻이다.

### loader

loader 속성은 자바스크립트파일이 아닌 웹자원(html,css,img,font..)를 변환할수 있도록 도와주는 속성입니다.

```
module.exports = {
  module: {
    rules: []
  }
}
```

예제파일을 통해 사용예제를 알아보자.

```
//app.js
import './common.css'

console.log('css loaded')

/* common.css */
p {
  color: blue;
}
```

만약에 entry가 app.js라고 가정하자. Output만 설정하고 빌드를 하게됬을때

웹팩은 module parse failed 오류를 보낸다.

웹팩이 css파일을 이해하지 못하니 모듈에 css를 이해하기위한 로더를 추가해달라는 뜻이다.

### loader 적용하기

위의 예처럼 Css를 적용하기 위해선 Css-loader라는것이 필요하다

`npm i css-loader` 설치후 아래와같이 수정해주자

```
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      },
      {
        test: /\.ts$/, 
        use: 'ts-loader'
      } ....
    ]
  }
}
```

module에 rules라는 배열에 객체가 생겼는데 각각의 역할은 다음과 같다.

- test : 로더를 적용할 파일 (일반적으로 정규표현식 사용)
- use : 해당파일에 적용할 로더의 이름

### loader의 적용순서

특정 파일에 여러개의 로더가 적용된다면. 그 순서를 주의해서 적어야합니다.

순서는 <---- 오른쪽에서 왼쪽순으로 적용됩니다.

예를들어 css의 scss로더를 적용하기위해선 

```
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ['css-loader', 'sass-loader']
    }
  ]
}
```

위와같이 sass로더로 scss파일을 Css로 변환한뒤 css로 적용하는 코드입니다.

대표적인 babel , sass webpack설정은 아래와같다
```
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
====
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          "style-loader",
          // Translates CSS into CommonJS
          "css-loader",
          // Compiles Sass to CSS
          "sass-loader",
        ],
      },
    ],
  },
};
Finally run webpack via your preferred method.
```

## plugin

플러그인은 웹팩의 동작에 추가적 기능을 제공하는 속성입니다.

로더가 파일을 해석하고 변환하는 과정이라면 플러그인은 해당 결과물의 형태를 바꾸는 역할을 한다고 보면됩니다.

```
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]
}
```
- HtmlWebpackPlugin : 웹팩으로 빌드한 결과물로 HTML 파일을 생성해주는 플러그인
- ProgressPlugin : 웹팩의 빌드 진행율을 표시해주는 플러그인
