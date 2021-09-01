# Loaders

로더는 모듈의 소스 코드에 변경 사항을 적용한다.

파일을 import하거나 "로드"할 때 별도의 전처리가 가능하다.

예시

- CSS
- typescript

로더는 다른 빌드 도구의 task와 유사하며 프론트엔드 빌드 단계를 처리하는 강력한 방법을 제공하기도 한다.

```javascript
// webpack.config.js

module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: "css-loader" },
      { test: /\.ts$/, use: "ts-loader" },
    ],
  },
};
```

## Using Loaders

로더를 사용하는 두 가지 방법이 있다.

- 설정 (추천): webpack.config.js 파일에 지정합니다.
- 인라인: 각 import 문에서 명확하게 지정합니다.

```javascript
// 설정
use: [
  // [style-loader](/loaders/style-loader)
  { loader: "style-loader" },
  // [css-loader](/loaders/css-loader)
  {
    loader: "css-loader",
    options: {
      modules: true,
    },
  },
  // [sass-loader](/loaders/sass-loader)
  { loader: "sass-loader" },
];

// inline
import Styles from "style-loader!css-loader?modules!./styles.css";
```

## Configuration

module.rules를 사용해 webpack 설정 내에 여러 개의 로더를 지정할 수 있다.

로더는 오른쪽에서 왼쪽으로 (또는 아래에서 위로) 평가/실행된다.

몇가지 중요한 특징들은 다음 Loader Feature에 정의한다.

```javascript
// sass-loader로 실행이 시작되고, css-loader로 이어지며 마지막으로 style-loader로 끝
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: "style-loader" },
          // [css-loader](/loaders/css-loader)
          {
            loader: "css-loader",
            options: {
              modules: true,
            },
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: "sass-loader" },
        ],
      },
    ],
  },
};
```

## Loader Features

- 로더는 연결할 수 있습니다. 체인의 각 로더는 처리된 리소스에 변환을 적용합니다.
- 로더는 동기식 또는 비동기식일 수 있습니다.
- 로더는 Node.js에서 실행되며 Node.js에서 가능한 모든 작업을 수행 할 수 있습니다.
- 로더는 options 객체로 구성 할 수 있습니다. (여전히 query 파라미터를 사용하여 옵션을 설정할 수 있지만 권장하지 않음)
- 일반 모듈은 loader 필드가 있는 package.json을 통해 main 및 로더를 내보낼 수 있습니다.
- 플러그인은 로더에 더 많은 기능을 제공 할 수 있습니다.
- 로더는 추가로 임의의 파일을 생성할 수 있습니다.

## Resolving Loaders

로더는 표준 모듈 해석을 따르며 대부분의 경우 모듈 경로에서 로드된다. (node_modules에서 가져옴)

npm module

네이밍 컨벤션은 일반적으로 xxx-loader(예 :json-loader)이다.
