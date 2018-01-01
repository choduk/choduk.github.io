---
title: React boilerplate 만들기
date: 2018-01-01
categories:
  - Develop
  - Front-End
tags:
  - env
  - react
  - babel
  - webpack
---

## React + babel + webpack 으로 boilerplate 생성 정리

> 프론트 알 못이라... 이번 기회에 각 용어와 셋팅 방법을 정리해보려고 한다 

#### 목차
 - 들어가기 전에..
 - 프로젝트 생성
 - webpack 설치
 - bebel 설치
 - webpack 설정
 - react
 - webpack plugin 추가

---
### 들어가기 전에..

1. webpack 이란?
 - js 파일을 하나로 번들링 해준다 (웹페이지 요청시 request 숫자를 줄일 수 있다)
 - npm 패키지를 Front-end에서 사용할 수 있다
 - ES6, ES7 등 더 많은 기능을 쓸 수 있다
 - 불필요한 파일을 빼고 번들링이 가능하다
 - less, scss 등을 사용할 수 있다 (css로 변경 가능)
 - Hot Module Replacement 를 사용할 수 있다
 - 어떤 파일이던 js파일에 넣어준다
 
2. babel 이란?
 - ES6, ES7으로 작성된 코드들을 ES5코드로 transpiling 해주는것
 - 다양한 모듈을 담는 역할을 하며, 코드를 transpiling 하기위해 작은 모듈(presets)을 사용한다
 - `babel-cli` : transpiling 수행
 - `babel-register` : 각 모듈을 결합할 때 사용 (production 용이 아니다)
 - `.babelrc` : 바벨 설정 파일(설정하지 않으면 단순히 파일을 옮기는 작업만 수행)
 - `babel-polyfill` : ES6 환경 제공
 - `babel-plugins` : babel에 여러 transform-plugin을 추가, `.babelrc`에서 설정하여도 되고 webpack에서 설정해도 된다
  
> transpiling 이란? 한 언어로 작성된 소스코드를 비슷한 수준의 추상화를 가진 다른언어로 변환하는 방법 (ex: typescript -> javascript)

즉 ES6/ES7으로 작성한 js파일을 babel을 통해 transpiling 되고, webpack이 이를 하나의 js파일로 번들링 해준다

3. npm 이란?
 - Node Package Modules 의 약자
 - node.js의 패키지 관리툴로서 자바스크립트로 패키지된 모듈들을 쉽게 공유할 수 있도록 도와주는 오픈소스 프로젝트
 - 개발자가 어떤 패키지를 쉽게 설치하고 퍼블리싱 할 수 있는 command line client

---
### 프로젝트 생성

(npm은 pc에 설치되어있다고 가정하고 진행)
비어있는 boilerplate `폴더 생성`후 `npm`을 init한다
```shell
$ mkdir react-boilerplate && cd react-boilerplate
$ npm init -y
```

---
### webpack 설치

위에서 생성한 폴더에 `npm`을 이용해 `webpack`을 설치하고 webpack 설정 파일(`webpack.config.js`)을 생성한다
```shell
$ npm i webpack --save-dev
$ touch webpack.config.js
```

---
### babel 설치

마찬가지로 `npm`을 이용해 `babel`과 관련된 코어모듈을 함께 설치하고 babel 설정파일(`.babelrc`)을 생성한다
```shell
$ npm i babel-loader babel-core babel-preset-env babel-preset-react --save-dev
$ touch .babelrc
```

> 주의! babel-preset-es2015는 deprecated 되었으므로 babel-preset-env를 쓰자

위에 만든 `.babelrc`파일에 다음과 같이 설정한다
```
{
  "presets": ["env", "react"]
}
```

---
### webpack 설정

아까 위에서 만든 `webpack.config.js`파일을 열어서 다음과 같이 설정한다
```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: ['./src/app/App.js'],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: ['/node_modules/']
      }
    ]
  }
};
```

위 `webpack.config.js`설정에서 entry point를 `./src/app.js`에 만들었으므로, 해당 파일을 하나 만들자

```shell
$ mkdir -p src/app && touch ./src/app/App.js
```

여기까지 만든 이후, `webpack`이 정상 작동을 하는지 테스트 한번 해보자
`package.json`파일을 열어서 다음과 같이 추가한다

```json
// package.json
{
  // ...생략
  "scripts": {
    "build": "webpack"
  }
  // ...생략
}
```

이후 아래를 명령어를 입력했을때 `webpack`설정에 적혀있듯이, `dist`폴더 하단에 `bundle.js`로 정상적으로 만들어 지는지 확인하자
```shell
$ npm run build
```

---
### React

위와 마찬가지로 `npm`을 이용하여 `react`와 `react-dom`을 설치한다

```shell
$ npm i react react-dom --save-dev
```

이제 react를 구성하기 위한 간단한 Hello App을 만든다

```shell
$ mkdir public && touch public/index.html
```

```html
<!- public/index.html ->
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>React App</title>
</head>
<body>
<div id="container"></div>
</body>
</html>
```

```javascript
// src/app/App.js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class App extends Component {
  constructor() {
    super();
    this.state = {
      title: "Hello App"
    };
  }
  render() {
    return (
      <h3>{this.state.title}</h3>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('container'));
```

---
### webpack plugin 추가

##### HTML webpack plugin

`public/index.html`에 자동으로 `<script src="bundle.js"></script>` 태그를 넣어 줄 수 있도록 plugin을 추가한다

```shell
$ npm i html-webpack-plugin html-loader --save-dev
```

`webpack.config.js` 파일 하단에 플러그인을 넣어준다
```javascript
// webpack.config.js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
  // .. 생략 
  ,
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "./index.html"
    })
  ]
}
```

이후 `npm run build` 명령어를 실행하여, `dist`폴더 하단에 `bundle.js`와 `index.html`이 생성되는지 확인하고 `index.html`에 `<script type="text/javascript" src="bundle.js"></script>` 가 들어가 있는지 확인하자


##### webpack Dev Server

간편한 개발을 위해 dev server 플러그인을 설정한다

```shell
$ npm i webpack-dev-server --save-dev
```

`webpack.config.js` 에 devServer 설정을, `package.json`엔 실행 스크립트를 추가한다

```javascript
// webpack.config.js

const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
  // .. 생략 
  devServer: {
    port: 3000,
    contentBase: "./dist"
  },
  // .. 생략
}
```

```json
// package.json
{
  // ...생략
  "scripts": {
    "start": "webpack-dev-server",
    "build": "webpack"
  }
  // ...생략
}
```

`npm run start`로 서버를 실행하고, 브라우저에서 `localhost:3000`으로 접속하여 Hello App이 뜨는걸 확인
 
## 참고자료
 - [react-webpack-babel](https://www.valentinog.com/blog/react-webpack-babel/)
 - [kleopetrov blog](http://kleopetrov.me/2016/03/18/everything-about-babel/)
 - [webpack3 설정](https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d)
 - [react directory structure](https://marmelab.com/blog/2015/12/17/react-directory-structure.html)
