# Concept

webpack은 모던 Javascript 정적 모듈 번들러다. 또한 webpack은 React 커뮤니티에서 채택한 주요 빌드 도구이다.

`webpack 4.0.0` 버전 이후로는 별도의 설정 파일을 필요로 하지 않으나 사용자 요구에 따라 설정이 가능하다.

```javascript
const defaultSetting = {
  entry: "./src/index.js",
  output: "./dist/main.js",
};
```

## Entry

webpack이 내부의 디펜던시 그래프를 생성하기 위해 사용해야 하는 모듈. (시작점)

엔트리포인트가 직간접적으로 의존하는 다른 모듈과 라이브러리를 찾아냄.

여러개의 엔트리포인트 지정 가능

## Output

생성된 번들을 내보낼 위치, 파일의 이름을 지정하는 방법을 webpack아 알려줌.

- 기본 출력 파일 : `./dist/main.js`
- 생성된 기타 파일 : `./dist` 폴더

```javascript
const path = require("path");

module.exports = {
  entry: "./path/to/my/entry/file.js",
  output: {
    path: path.resolve(__dirname, "dist"), // 내보낼 위치
    filename: "my-first-webpack.bundle.js", // 번들의 이름
  },
};
```

## Loaders

webpack은 기본적으로 JavaScript JSON 파일만 이해할 수 있다.

로더를 사용하면 다른 유형의 파일(이미지, CSS등)을 처리하거나, 그들을 유효한 모듈로 변환 하여 애플리케이션에서 사용하거나 디펜던시 그래프에 추가한다.

webpack 설정에서 로더는 다음 2가지 속성을 지닌다.

- 변환이 필요한 파일(들)을 식별하는 `test` 속성
- 변환을 수행하는데 사용되는 로더를 가리키는 `use` 속성

```javascript
const path = require("path");

module.exports = {
  output: {
    /* ... */
  },
  module: {
    // import, require에서 '.txt' 파일을 발견하면 번들에 추가하기 전에 raw-loader를 사용하여 변환
    rules: [{ test: /\.txt$/, use: "raw-loader" }],
  },
};
```

webpack 설정에서 규칙을 정의할 때 `rules`가 아닌 `module.rules` 아래에 정의한다.

## Plugins

Loader는 특정 유형의 모듈을 변환하는데 사용.

Plugins는 bundle을 최적화 하거나 asset을 관리하고 환경변수등을 주입하는 광범위한 작업 수행

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin"); // npm을 통해 설치
const webpack = require("webpack"); // 내장 plugin에 접근하는 데 사용

module.exports = {
  module: {
    // txt 파일의 경우는 raw-loader를 이용해 변환후 번들에 추가
    rules: [{ test: /\.txt$/, use: "raw-loader" }],
  },
  // 생성된 모든 번들을 자동으로 삽입하여 애플리케이션용 HTML 파일을 생성
  plugins: [new HtmlWebpackPlugin({ template: "./src/index.html" })],
};
```

## Mode

mode를 `development`, `production` 또는 `none`으로 설정시에 webpack에 내장된 환경별 최적화를 활성화 할 수 있음.

기본 값은 `production`.

- `development` : `DefinePlugin`의 `process.env.NODE_ENV`를 `development`로 설정합니다. 모듈과 청크에 유용한 이름을 사용할 수 있습니다.
- `production` : `DefinePlugin`의 `process.env.NODE_ENV`를 `production`으로 설정합니다. 모듈과 청크, `FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `TerserPlugin` 등에 대해 결정적 망글이름(mangled name)을 사용할 수 있습니다.
- `none` : 기본 최적화 옵션에서 제외

## Browser Compatibility

브라우저 호환성

webpack은 ES5가 호환되는 모든 브라우저를 지원한다. (IE9까지 지원함)

webpack에서는 `import`, `require.ensure`을 위한 `Promise`를 사용하기 때문

---

## 참고자료

### Dependency Graph (의존성 그래프)

하나의 파일이 다른 파일에 의존할 때마다 의존성으로 취급한다.

이를 통해 webpack은 이미지, 웹 폰트와 같은 "코드"가 아닌 asset들을 가져오고 애플리케이션에 의존성으로 제공할 수 있다.

webpack은 커맨드 라인, 설정 파일에 정의 된 모듈 목록에서 시작한다. (엔트리 포인트)

애플리케이션에서 필요한 모든 모듈을 포함하는 `의존성 그래프`를 재귀적으로 빌드한 다음에, 모든 모듈을 작은 수(보통 하나)의 번들로 묶는다.

### require

브라우저는 require()을 지원하지 않는다.

- require는 NodeJS에서 사용되고 있는 CommonJS 키워드.
- import는 ES6(ES2015)에서 새롭게 도입된 키워드.

[JavaScript 표준을 위한 움직임: CommonJS와 AMD](https://d2.naver.com/helloworld/12864)

### webpack의 중복된 모듈 시나리오

- [Finding and fixing duplicates in webpack with Inspectpack](https://formidable.com/blog/2018/finding-webpack-duplicates-with-inspectpack-plugin/)
- [Performance in Jira front-end: solving bundle duplicates with Webpack and yarn](https://www.atlassian.com/engineering/performance-in-jira-front-end-solving-bundle-duplicates-with-webpack-and-yarn)

### Tree shaking

import 문에서 필요한 요소만 가져옴

- [트리 쉐이킹으로 자바스크립트 페이로드 줄이기](https://ui.toast.com/weekly-pick/ko_20180716)

```javascript
// import the entire utils object with CommonJS
const utils = require("./utils");
const query = "Rollup";
// use the ajax method of the utils object
utils.ajax(`https://api.example.com?search=${query}`).then(handleResponse);
```

```javascript
// import the ajax function with an ES6 import statement
import { ajax } from "./utils";
const query = "Rollup";
// call the ajax function
ajax(`https://api.example.com?search=${query}`).then(handleResponse);
```

애플리케이션이 오래될수록 더 많은 디펜던시들이 추가될 수 있다. 복잡한 문제들을 위해 오래된 디펜던시들을 빼지만, 코드에서는 제거되지 못할 수 있다.

트리 쉐이킹은 정적 ES6 모듈의 특정 부분을 가져오는 import 구문의 이점을 사용해 이러한 문제를 해결할 수 있다.
