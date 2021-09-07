## Concepts

### 개요

- js용 정적 모듈 번들러
- 디펜던시 그래로 번들을 생성
- 4.0 이후로는 별도의 설정파일 필요없음

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); // npm을 통해 설치
const webpack = require('webpack'); // 내장 plugin에 접근하는 데 사용
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
  mode: 'production', // production, development, none
};
```



### Entry

- 디팬던시 그래프를 생성에 사용하는 모듈을 지정하는 설정
- 포인트 역할을 하며, entry가 지정한 경로를 통해 다른 모듈과 라이브러리를 찾아낸다.



### Output

- 번들을 내보낼 위치를 지정하는 설정
- filename과 path같은 추가 속성값을 가질 수 있다.



### Loaders

- 본래 웹팩은 js와 json만 이해하지만 로더를 통해 다른 유형의 파일도 처리or모듈화 가 가능해진다.
- 위의 예제에서는 module.rules를 통해 대상파일식별(test), 변환에 사용될 로더(use)를 사용했다.



### Plugins

- 로더 이외에, 번들 최적화, 에셋관리, 환경변수 주입과 같은 부가기능을 수행하게 해준다.



### Mode

- production, development, none중 하나를 선택하여 웹팩 내장환경을 설정할 수 있다.
- 디폴트는 production

