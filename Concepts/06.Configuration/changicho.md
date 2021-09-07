# Configuration

webpack의 설정 파일은 webpack 설정을 내보내는 JavaScript 파일이다.

따라서 webpack 설정이 정확히 동일한 경우는 거의 없다.

webpack 설정을 형식화하고 스타일을 지정하는 다양한 방법이 있다.

webpack은 표준 Node.js CommonJS 모듈이다.

- require(...)를 통해 다른 파일 가져오기
- require(...)를 통해 npm 유틸리티 사용하기
- `condition ? a : b` 연산자 같은 JavaScript 제어 흐름 표현식 사용하기
- 자주 사용되는 값에 상수 또는 변수 사용하기
- 설정 일부를 만들어 내는 함수 작성 및 실행하기

아래 사용 방법들은 피하자

- webpack CLI를 사용할 때, 자체 CLI를 작성하거나 --env를 사용하는 대신, CLI 인자에 접근하기
- 결정되지 않은 값을 내보내기 (webpack을 두 번 호출하면 동일한 출력 파일이 생성됩니다)
- 긴 설정 작성하기 (대신 설정을 여러 파일로 분할)

## Introductory Configuration

```javascript
// webpack.config.js

const path = require("path");

module.exports = {
  mode: "development",
  entry: "./foo.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "foo.bundle.js",
  },
};
```

## Multiple Targets

단일 설정을 객체, 함수 또는 promise로 export 하는 것과 함께,

다중 설정을 export 할 수 있다.

```javascript
module.exports = [
  {
    output: {
      filename: "./dist-amd.js",
      libraryTarget: "amd",
    },
    name: "amd",
    entry: "./app.js",
    mode: "production",
  },
  {
    output: {
      filename: "./dist-commonjs.js",
      libraryTarget: "commonjs",
    },
    name: "commonjs",
    entry: "./app.js",
    mode: "production",
  },
];
```

## Using other Configuration Languages

webpack은 다양한 프로그래밍 및 데이터 언어로 작성된 설정 파일을 허용한다.

- Typescript
- CoffeeScript
- Babel and JSX
