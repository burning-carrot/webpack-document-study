# Output

엔트리포인트의 파일들을 읽어들이고 컴파일된 파일들을 디스크에 쓰는 방법을 webpack에 알려준다.

엔트리포인트는 여러개일 수 있으나 output은 하나만 가능함에 유의한다.

```javascript
// webpack.config.js
module.exports = {
  output: {
    filename: "bundle.js",
  },
};
```

## Multiple Entry Points

하나 이상의 청크(아웃풋)을 생성할 경우 substitution(치환문법규칙)을 사용해 파일이 고유한 이름을 갖도록 해야 한다. (중복되면 안됨)

```javascript
module.exports = {
  entry: {
    app: "./src/app.js",
    search: "./src/search.js",
  },
  output: {
    filename: "[name].js",
    path: __dirname + "/dist",
  },
};

// writes to disk: ./dist/app.js, ./dist/search.js
```

## Advanced

애셋에서 CDN과 해시를 사용한 예제

```javascript
module.exports = {
  //...
  output: {
    path: "/home/proj/cdn/assets/[fullhash]",
    publicPath: "https://cdn.example.com/assets/[fullhash]/",
  },
};
```

출력 파일의 최종 publicPath를 컴파일 시점에 알 수 없을 경우 우선 공백으로 남겨둔다.

이후 런타임에 엔트리 포인트 파일의 **webpack_public_path**를 통해 동적으로 설정할 수 있습니다.

```javascript
__webpack_public_path__ = myRuntimePublicPath;

// rest of your application entry
```
