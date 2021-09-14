# Plugins

### 개요

- 로더가 할 수 없는 다른 작업을 수행한다.
  - 로더는 파일을 해석하고 변환하는 과정에 관여하고, 플러그인은 해당 결과물의 형태를 바꾸는 역할을 한다.
  - 의견 : 변환 중에 관여하냐 변환 후에 관여하냐의 차이인듯 하다.
- 웹팩의 기본적인 동작에 추가적인 기능을 제공한다.



### 구조

```javascript
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    // pluginName은 Camelcase로 작성, 상수를 사용하는게 좋음
    compiler.hooks.run.tap(pluginName, (compilation) => {
      console.log('The webpack build process is starting!');
    });
  }
}

module.exports = ConsoleLogOnBuildWebpackPlugin;
```

- 플러그인은 apply 메서드를 가지는 객체
- apply는 webpack 컴파일러에 의해 호출되어 전체 컴파일 생명주기에 접근할 수 있다.



#### 참고문서

- https://joshua1988.github.io/webpack-guide/concepts/plugin.html#plugin



### 사용법

#### Configuration

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //npm 으로 설치됨
const webpack = require('webpack'); //빌트인 플러그인에 접근하기 위함
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  plugins: [
    new webpack.ProgressPlugin(),
     // 웹팩 빌드 진행율을 보여준다.
    new HtmlWebpackPlugin({ template: './src/index.html' }),
    // 출력 번들파일을 포함하는 html 파일을 생성한다.
  ],
};
```

- 앞서 말했듯 플러그인은 객체이기 때문에 new 연산자를 사용해서 생성된 인스턴스를 지정한다.



### 주요 플러그인

- [split-chunks-plugin](https://webpack.js.org/plugins/split-chunks-plugin/)
  - chunk를 생성하는 과정에서 dependancy tree에서 중복되는 모듈을 최적화할 수 있다.
  - 웹팩3 이전까지 사용하던 CommonsChunkPlugin를 대신한다.
- [clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin)
  - 빌드 결과물 폴더를 자동으로 청소해주는 플러그인
- [image-webpack-loader](https://github.com/tcoopman/image-webpack-loader)
  - 웹팩용 이미지 로더 모듈
- [webpack-bundle-analyzer-plugin](https://github.com/webpack-contrib/webpack-bundle-analyzer)
  - 웹팩 결과물을 크기를 비롯한 정보들을 treemap으로 시각화해서 보여준다.
  - 단순히 번들링되 파일뿐만 아니라 사용되는 각 모듈과 해당 모듈 내부에서 사용되는 다른 모듈들의 크기를 의존성에 맞게 보여준다.

