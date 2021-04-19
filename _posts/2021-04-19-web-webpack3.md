---
layout: post
title:  "Webpack 실습"
subtitle: "Webpack 실습"
categories: web
tags: webpack
comments: true

---

## 간단한 파일구성을 통해 웹팩을 실습해보자.


```
index.html
====
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>웹팩실습</title>
</head>
<body>
    <header>
        <h3>css code split</h3>
    </header>
    <div>
        <p>
            this text should be colored with blue after injecting css bundle
        </p>
    </div>
    <script src="dist/bundle.js"></script>
</body>
</html>

src/index.js
===
import './base.css'

base.css
===
p {
  color : blue;
}

webpack.config.js
====
var path = require("path");
// var MiniCssExtractPlugin = require("mini-css-extract-plugin");

// css 여러파일을

module.exports = {
  mode: "none",
  entry: "./index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  //   plugins: [new MiniCssExtractPlugin()],
};
```

npm run build 를 하면 우리가 만든 js를 적용하여 html에 import하면 잘 적용된것을 볼수있다.

음 그런데 plugin의 minicssextractplugin()은 뭘까?

js에 포함(Import)된 css를 별도의 파일로 추출한다