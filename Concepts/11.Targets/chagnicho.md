# Targets

JavaScript는 서버와 클라이언트 모두 작성이 가능하다.

대략적인 환경은 다음과 같다.

- 서버 : Node
- 클라이언트 : 브라우저

webpack은 설정에서 다수의 배포대상을 설정할 수 있다.

webpack설정의 `target`은 `output.libraryTarget`과 다르다.

## Usage

다음과 같이 설정 파일에서 사용할 수 있다.

```javascript
// webpack.config.js

module.exports = {
  target: "node",
};
```

위 예제에서는 Node.js와 유사한 환경에서 사용할 수 있도록 컴파일한다.

(Node.js의 require를 사용하여 청크를 로드하고 fs나 path와 같은 모듈은 수정하지 않음)

## Multiple Targets

webpack에서는 한 개 이상의 문자열을 target 프로퍼티에 전달할 수 없다.

(하나의 문자열만 전달할 수 있음)

두개 이상의 개별 설정을 번들링해서 동일한 라이브러리를 여러 target으로 생성할 수 있다.

```javascript
// webpack.config.js;

const serverConfig = {
  target: "node",
  //…
};

const clientConfig = {
  target: "web", // <=== 기본값은 'web'이므로 생략 가능
  //…
};

module.exports = [serverConfig, clientConfig];
```

위 예제는 `output.path`, `output.filename`에 설정된 경로로 `파일명.js` 파일을 생성한다.
