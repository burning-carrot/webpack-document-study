# Entry Points

엔트리 포인트는 webpack이 내부의 디펜던시 그래프 를 생성하기 위해 사용해야 하는 모듈이다.

엔트리포인트가 의존하는 다른 모듈과 라이브러리들을 찾아낸다.

entry 속성을 정의하는 방법은 여러가지가 존재한다.

기본값은 `./src/index.js`이다.

## Single Entry (Shorthand) Syntax

단일 엔트리포인트 축약 표기법

```javascript
// webpack.config.js

module.exports = {
  entry: "./path/to/my/entry/file.js",
};
// is same with
module.exports = {
  entry: {
    main: "./path/to/my/entry/file.js",
  },
};
```

엔트리 포인트는 다중으로 설정할 수 있다. (다중 엔트리 포인트)

```javascript
// webpack.config.js

module.exports = {
  entry: ["./src/file_1.js", "./src/file_2.js"],
};
```

## Object Syntax

객체 표기법

```javascript
// webpack.config.js

// Usage: entry: { <entryChunkName> string | [string] } | {
module.exports = {
  entry: {
    app: "./src/app.js",
    adminApp: "./src/adminApp.js",
  },
};
```

웹앱에서 엔트리 포인트를 정의하는 가장 확장 가능한 방법이다.

## EntryDescription object

엔트리 포인트 설명이 있는 객체

- dependOn: 현재 엔트리 포인트가 의존하는 엔트리 포인트. 이 엔트리 포인트를 로드하기 전에 로드해야 합니다.
- filename: 디스크에 있는 각 출력 파일의 이름을 지정합니다.
- import: 시작 시 로드되는 모듈입니다.
- library: 현재 엔트리에서 라이브러리를 번들링하려면 라이브러리 옵션을 지정합니다.
- runtime: 런타임 청크의 이름입니다. 설정되면 이 이름의 런타임 청크가 생성되고 그렇지 않으면 기존 엔트리 포인트의 이름이 사용됩니다.
- publicPath: 브라우저에서 참조할 때 이 엔트리의 출력 파일에 대한 공용 URL 주소를 지정하세요.

runtime과 dependOn은 단일 엔트리에 함께 사용해서는 안된다.

또한 runtime의 경우 실행시에 만들어지는 엔트리포인트이므로 기존 엔트리 포인트를 가리키면 안된다.

dependOn의 경우 순환참조를 하면 안된다.

```javascript
// webpack.config.js

module.exports = {
  entry: {
    a2: "dependingfile.js",
    b2: {
      dependOn: "a2",
      import: "./src/app.js",
    },
  },
};
```

## Scenarios

### Separate App and Vendor Entries

```javascript
// webpack.config.js
module.exports = {
  entry: {
    main: "./src/app.js",
    vendor: "./src/vendor.js",
  },
};

// webpack.prod.js
module.exports = {
  output: {
    filename: "[name].[contenthash].bundle.js",
  },
};

// webpack.dev.js
module.exports = {
  output: {
    filename: "[name].bundle.js",
  },
};
```

vendor.js 내에서 수정되지 않은 필수 라이브러리 또는 파일들을 담아둔다. (Bootstrap, jQuery, images, 등)

이들은 자체 청크로 함께 번들 제공되며 브라우저가 별도로 캐시하므로 로드 시간을 줄일 수 있다.

### Multi Page Application

```javascript
// webpack.config.js
module.exports = {
  entry: {
    pageOne: "./src/pageOne/index.js",
    pageTwo: "./src/pageTwo/index.js",
    pageThree: "./src/pageThree/index.js",
  },
};
```

엔트리 포인트들 사이에 코드/모듈을 많이 재사용하는 멀티 페이지 애플리케이션은 엔트리 포인트 수가 증가함에 따라 이런 기법이 유리할 수 있다.
