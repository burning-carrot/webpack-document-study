# Plugins

webpack 자체는 webpack 설정에서 사용하는 것과 동일한 플러그인 시스템으로 구축되어 있다.

로더가 할 수 없는 다른 작업을 수행할 목적으로 제공.

## Anatomy (해부해보기)

webpack 플러그인은 apply 메서드를 가지고 있는 객체이다.

`Function.prototype.apply()`

apply 메소드는 webpack의 컴파일러에 의해서 호출되며, 전체 컴파일 라이프사이클에 접근할 수 있다.

```javascript
// 컴파일러 훅의 탭 메서드의 첫번째 파라미터
// 플러그인 이름의 Camelcase 버전으로 작성해야 한다.
// 모든 훅에서 재사용 할 수 있도록 하기 위해서 상수를 사용하는 것이 좋다.
const pluginName = "ConsoleLogOnBuildWebpackPlugin";

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, (compilation) => {
      console.log("The webpack build process is starting!");
    });
  }
}

module.exports = ConsoleLogOnBuildWebpackPlugin;
```

## Usage

플러그인은 매개변수 및 옵션을 사용할 수 있다.

webpack 설정에서 plugins 속성에 새로운 인스턴스로 전달한다. (new로 생성)

webpack을 사용하는 방법에 따라 여러 가지 방법으로 플러그인을 사용 할 수 있다.

### Configuration

```javascript
// webpack.config.js

const HtmlWebpackPlugin = require("html-webpack-plugin"); //npm 으로 설치됨
const webpack = require("webpack"); //빌트인 플러그인에 접근하기 위함
const path = require("path");

module.exports = {
  entry: "./path/to/my/entry/file.js",
  output: {
    filename: "my-first-webpack.bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: "babel-loader",
      },
    ],
  },
  plugins: [
    new webpack.ProgressPlugin(), // 컴파일하는 동안 리포트를 사용자 정의로 생성 할 수 있다.
    new HtmlWebpackPlugin({ template: "./src/index.html" }), // script를 사용하여 my-first-webpack.bundle.js 파일을 포함하는 HTML 파일을 생성
  ],
};
```

## Node API

Node API를 사용할 때 설정의 플러그인 속성을 통해 플러그인을 전달 할 수도 있다.

```javascript
// some-node-script.js

const webpack = require("webpack"); //webpack 런타임에 접근하기 위함
const configuration = require("./webpack.config.js");

let compiler = webpack(configuration);

new webpack.ProgressPlugin().apply(compiler);

compiler.run(function (err, stats) {
  // ...
});
```
