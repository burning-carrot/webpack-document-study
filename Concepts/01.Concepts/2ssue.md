# [Webpack Concepts](https://webpack.kr/concepts/) #

- 모던 JavaScript 애플리케이션을 위한 정적 모듈 번들러
- Webpack이 애플리케이션을 처리하는 방식
  1. 내부적으로 프로젝트에 필요한 모든 [모듈](#Modules)을 매핑
  1. 하나 이상의 번들을 생성하는 [디펜던시 그래프](#Dependency-Graph)를 만듦
- 버전 4.0.0 이후로는 별도의 설정파일이 필요하진 않지만, [커스텀](https://webpack.kr/configuration/) 가능

## 핵심 개념 ##

### [Entry (엔트리) == Entry Points (엔트리 포인트)](https://webpack.kr/concepts/entry-points) ###

webpack이 내부의 [디펜던시 그래프](#Dependency-Graph)를 생성하기 위해 사용해야하는 모듈. 엔트리포인트가 의존하는 다른 모듈과 라이브러리를 찾아낸다.

- **webpack.config.js**

```js
module.exports = {
    /**
    * default: `./src/index.js`
    */
    entry: './path/to/my/entry/file.js',
}
```

### [Output (출력)](https://webpack.kr/concepts/output) ###

생성된 번들을 내보낼 위치, 파일의 이름을 지정하는 법을 webpack에 알려주는 역할.

- **webpack.config.js**

```js
const path = require('path');

module.exports = {
    entry: './path/to/my/entry/file.js',
    /**
    * Default configuration
    * 
    * output: {
    *    path: ./dist
    *    filename: main.js
    * }
    *
    * more detail: https://webpack.kr/configuration/output
    */
    output: {
        path: path.resolve(__dirname, 'dist'), // 번들을 내보낼 위치
        filename: 'my-first-webpack.bundle.js', // 번들 이름
    },
};
```

### [Loaders (로더)](https://webpack.kr/concepts/loaders) ###

webpack에서 네이티브(Javascript, JSON)가 아닌 모듈을 어떻게 처리하고, 이러한 의존성을 번들에 포함할지 정의하는데 사용되는 기능.
> cf. webpack은 기본적으로 JavaScript, JSON만 이해할 수 있음

다른 유형의 파일을 처리하거나 유효한 모듈로 변환하고, 이를 애플리케이션에서 사용하거나 디펜던시 그래프에 추가함.

ex) css 파일을 직접 `import` 하거나, TypeScript를 JavaScript로 변환하는 등

- 로더의 webpack 설정 두가지 속성
  1. `test`: 변환이 필요한 파일들을 식별
  1. `use`: 변환을 수행하는 데 사용되는 로더를 가리킴

- **webpack.config.js**

```js
const path = require('path');

module.exports = {
    output: {
        filename: 'my-first-webpack.bundle.js',
    },
    module: {
        /**
        * require(), import 에서 `.txt` 파일 경로를 발견하면, (test)
        * 번들에 추가하기 전 raw-loader를 사용해 변환해라 (use)
        */
        rules: [{test: /\.txt$/, use: 'raw-loader'}], 
    },
};
```

### [Plugins (플러그인)](https://webpack.kr/concepts/plugins) ###

번들을 최적화하거나 에셋을 관리하고, 환경 변수 주입등과 같이 광범위한 작업을 수행할 수 있도록 돕는 기능. 로더가 할 수 없는 다른 작업을 수행할 목적으로 제공됨.

- 플러그인 사용하기
  - `require()`를 통해 플러그인 요청, `plugins` 배열에 추가
  - `new` 연산자로 인스턴스를 생성해서 만들어야함
    - 다른 목적으로 플러그인을 여러번 사용하도록 할 수 있기 때문
    - 옵션을 통해 기능 지정 가능

- **webpack.config.js**

```js
const HtmlWebpackPlugin = require('html-webpack-plugin') // npm install
const webpack = require('webpack') // 내장 plugin에 접근하는 데 사용

module.exports = {
    module : {
        rules: [{test: /\.txt$/, use: 'raw-loader'}],
    },
    plugins: [new HtmlWebpackPlugin({template: './src/index.html'})], // 생성된 모든 번들을 자동으로 삽입해 애플리케이션용 HTML 파일 생성
};
```

#### 추가로 참고하면 좋을 것 ####

- [플러그인 인터페이스](https://webpack.kr/api/plugins)
- [설치없이 사용가능한 플러그인](https://webpack.kr/plugins)

### Mode (모드) ###

- **webpack.config.js**

```js
module.exports = {
    /**
    * default: production
    * to-be: development, production, none
    */
    mode: 'production',
}
```

webpack에 내장된 [환경별 최적화](https://webpack.kr/configuration/mode)를 활성화 할 수 있음.

|옵션|설명|비고|
|---|---|---|
|`development`|[`DefinePlugin`](#DefinePlugin)의 `process.env.NODE_ENV`를 `development`로 설정. 모듈과 청크에 유용한 이름을 사용할 수 있음|-|
|`production`|`DefinePlugin`의 `process.env.NODE_ENV`를 `production`으로 설정. 모듈과 청크, `FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `TerserPlugin` 등에 대해 결정적 [mangled name](https://en.wikipedia.org/wiki/Name_mangling)(컴파일러가 임의로 변경한 변수의 이름)을 사용할 수 있음|default|
|`none`|기본 최적화 옵션에서 제외|-|

### Browser Compatibility (브라우저 호환성) ###

- [ES5](https://kangax.github.io/compat-table/es5/)가 호환되는 모든 브라우저를 지원 (IE8 이하 X)

> cf. [`import()/require.ensure()`(= Dynamic Imports)](https://webpack.kr/guides/code-splitting/#dynamic-imports)를 위해 `Promise`가 필요하며, 구형 브라우저(IE 11 이하)를 지원하려면 이러한 표현식을 사용하기 전 [폴리필을 로드](https://webpack.kr/guides/shimming/)해야함

## 참고해보면 좋은 자료 ##

- 모듈 번들러의 개념과 내부에서 동작하는 방식
  - [수동으로 애플리케이션을 번들링하기](https://www.youtube.com/watch?v=UNMkLHzofQI)
  - [간단한 모듈 번들러 라이브 코딩](https://www.youtube.com/watch?v=Gc9-7PBqOC8)
  - [간단한 모듈 번들러에 대한 자세한 설명](https://github.com/ronami/minipack)

---

## 자세히 보기 ##

### [Modules](https://webpack.kr/concepts/modules/) ###

모듈형 프로그래밍에서 개발자는 `모듈`이라는 개별 기능으로 프로그램을 나눈다.

각 모듈은 전체 프로그램보다 영향범위가 좁기 때문에, 검증과 디버깅/테스트가 간단하다. 잘 작성된 모듈은 견고한 추상화와 캡슐화의 경계를 만들기 때문에 각 모듈은 전체 애플리케이션에서 일관성 있는 설계와 명확한 목적을 가질 수 있다.

webpack은 Node.js 등과 같은 시스템에서 얻은 교훈을 바탕으로 제작되어 프로젝트의 모든 파일에 모듈의 개념을 사용한다.

- webpack 모듈의 다양한 의존성 표현 방식 (예시)
  - [ES2015 `import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
  - [CommonJS](http://www.commonjs.org/specs/modules/1.0/) `require()`
  - [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) `define`&`require`
  - css/sass/less [`@import`](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)
  - stylesheet `url(...)` / HTML `<img src="...">`
- webpack이 지원하는 기본 모듈
  - [ECMAScript](https://webpack.kr/guides/ecma-script-modules)
  - CommonJS
  - AMD
  - [Assets](https://webpack.kr/guides/asset-modules)
  - WebAssembly
- 그 외 (로더를 통한 전처리기 지원)
  - [CoffeeScript](http://coffeescript.org/)
  - [TypeScript](https://www.typescriptlang.org/)
  - [ESNext (Babel)](https://babeljs.io/)
  - [Sass](http://sass-lang.com/)
  - [Less](http://lesscss.org/)
  - [Stylus](http://stylus-lang.com/)
  - [Elm](https://elm-lang.org/)
  - [...](https://webpack.kr/loaders)

#### 더 읽어보면 좋을 것 ####

- [JavaScript Module Systems Showdown](https://auth0.com/blog/javascript-module-systems-showdown/)

### [Dependency Graph](https://webpack.kr/concepts/dependency-graph/) ###

webpack은 하나의 파일이 다른 파일에 의존할 때마다 의존성으로 취급한다. 이를 통해 코드가 아닌 에셋(이미지 또는 웹 폰트와 같은)을 가져와 애플리케이션에 의존성으로 제공할 수 있다.

webpack은 애플리케이션을 처리할 때 커맨드라인이나 설정파일에 정의된 모듈 목록에서 시작한다. [엔트리 포인트(configuration에서 entry로 지정된 파일)](https://webpack.kr/concepts/entry-points/)부터 애플리케이션에서 필요한 모든 모듈을 포함하는 디펜던시 그래프를 재귀적으로 빌드한 다음, 모든 모듈을 브라우저에 의해 로드되는 번들(보통 하나의 파일)로 묶는다.

### [DefinePlugin](https://webpack.kr/plugins/define-plugin/) ###

`DefinePlugin`을 사용하면 컴파일 타임에 구성할 수 있는 전역 상수를 만들 수 있으며, 개발 빌드와 프로덕션 빌드간에 서로 다른 동작을 하고 싶을 때 유용하다. 

개발/프로덕션 빌드 환경을 설정할 때 `DefinePlugin`이 유용한데, 개발 빌드에서 로깅을 수행하지만, 프로덕션 빌드에서는 수행하지 않는 경우 전역 상수를 사용해 로깅을 수행할 지 여부를 결정할 수 있기 때문이다.

