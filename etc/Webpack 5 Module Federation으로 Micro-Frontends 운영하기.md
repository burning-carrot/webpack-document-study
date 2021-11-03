# Webpack 5 Module Federation으로 Micro-Frontends 운영하기

[youtube](https://www.youtube.com/watch?v=3vGNuDCOMN4)

기존 상황 : 하나의 앱에 10개가 넘는 기능을 제공하고 있었음

## 기존 아키텍처의 문제점

엄청나게 많은 서비스를 모놀리식 구조로 관리하고 있었음

모놀리식 패키지 구조

### 피처간 의존성 문제

특정 피저에 버그가 발생 > 다른 피처에도 문제가 발생

서로 의존하고 이쓴ㄴ 피처들 사이에 발생한 문제의 원인이 어디에 있는지 파악하기 힘들다. (의존 트리를 하나씩 타고 디버깅해야함)

특정 피처에 새로운것을 추가하거나 수정할 때 의존성이 있는 다른 피처에 영향이 감

### 비효율적인 빌드 및 배포

하나의 기능에 버그 발생, 서버 인터페이스 변경 대응 등으로 인해 코드 수정이 발생하면

변경되지 않은 코드들도 빌드에 포함

기능이 많아지면 빌드 시간만 수십분

여러 피처를 여러 개발하거나 수정할 때 conflict 발생함 (조직이 커지고 나눠지면 일상이됨)

### 동일한 기능, 여러번의 개발

동일한 기능을 여러 앱에서 동일하게 제공

동일한 비즈니스 로직을 앱마다 각각 구현해야 하는 이슈 > 리소스 낭비

## 새로운 아키텍처로의 전환

만약에 이럴 수 있다면 어떨까?

- 스크럼 별 독립적인 개발 및 배포
- 다른 피처에 영향받지 않는 독립 패키지
- 단일 비즈니스 로직을 여러 환경에서 사용하기

이를 위해 Micro-Frontends Architecture를 도입하는건 어떨까

독립적으로 운영 및 개발이 가능한 프론트엔드 애플리케이션들로 `더 큰 하나의 전체 애플리케이션`을 구성하는 아카텍처

다음과 같은 플로우로 운영될 수 있다.

하나의 서비스는 하나의 독립된 팀에서 독립적으로 운영되고 개발할 수 있다. CI/CD 파이프라인을 통해 빌드되고 배포된다.

각 팀마다 개발한 개발 애플리케이션이 프로덕트에 합쳐진다.

기능

- 개발자 : 독립적인 객체
- 사용자 : 바운더리 없는 서비스

### Micro Frontends 를 구현하기 위해 고려했던 방법들

- Build-Time Integration (통합) : 빌드시간을 줄여야함
- Run-Time Native WebView Integration
- Run-Time Bundle Integration
- Run-Time iframe Integration

빌드타임에 통합, 런타임에 통합

### Native Webview 이용하기

가장 이상적이라고 생각되는 통합 방식임. 세션관리가 용이하고 native 의 자원을 이용할 수 있다. (bridge를 이용)

초기 개발공수와 인력 구하기가 힘듬

### iframe 이용하기

iframe 태그를 이용한 방법. 가장 전통적이고 널리알려짐

보안, 성능, 안정성 이슈 (취약함)

앱 간의 통신 인터페이스를 구현해야함 (postMessage)

딥링크나 라우팅에 대한 고민 (모바일, 하드웨어, history 처리)

### Runtime 통합 이용하기

```html
// 다음과 같은 구조의 스크립트
<script src="https:// ... bundle1.js"></script>
<script src="https:// ... bundle2.js"></script>
<script src="https:// ... bundle3.js"></script>
```

```javascript
// 이후 각 번들마다 init을 수행한다. (번들 등록)
init(window.mf.app1);

// 이후 라우터에서 번들을 동작시킴
route = {
  "/path1": () => run(window.mf.app1),
  "/path2": () => run(window.mf.app2),
  "/path3": () => run(window.mf.app3),
};
```

이러한 전통적인 방식을 채택한 라이브러리는 [single-spa](https://single-spa.js.org/)가 있다.

런타임에서 번들을 통합하는 방법으로 webpack5의 module federation이 있음

## Module Federation

런타임에 통합된 각 애플리케이션들이 서로 코드를 공유한다.

### 특징 1 - UI Component

다른 기술들은 runtime에 UI를 통합하는 기능이다. (라우트 기반이건, SPA 레이아웃 갱신 기반이건)

비즈니스 로직, 상태, JSON, Assets, styles까지 공유가 가능하다.

- 웹팩이 다루는 모든걸 공유할 수 있다.
- Any Javascript Runtimes (NodeJS, web worker, 일렉트론)

모든 애플리케이션 환경을 Module Federation으로 구성한다면?

필요한 부분들만 remote에 앱으로 띄운다.

이후 host에서 런타임에서 이를 가져와 사용한다.

요약하면 `독립적인 특정 애플리케이션에 있는 코드들을 모듈이라는 단위로서 다른 애플리케이션에 공유하고 또 공유 받을 수 있는 기능.`

### 런타임에서 코드를 공유하는 것의 가장 큰 의미

배포된 코드가 실시간으로 반영된다.

module federation을 사용하기 위한 convention이자 절차

```javascript
// index.js
import("./entry") // dynamic import

// entry.js
import React from "react";
import ReactDom from "react-dom";
import App from "./App";

ReactDOM.render(<App/>, document.get ...);
```

공식문서에서는 dynamic import를 통해 lazy하게 코드를 불러오게 하면

webpack이 앱을 실행하기 전에 module federation을 통해서 통합되는 remote의 앱들을 로드하는 과정을 수행한다.

Module Federation을 사용하기 위해선 `webpack.config.js`에서 플러그인을 사용해야 한다.

[webpack 공식문서 - module-federation-plugin](https://webpack.js.org/plugins/module-federation-plugin/)

```javascript
// expose 하는 쪽 webpack.config.js
const deps = require("./package.json").dependencies;

module.exports = {
  // ...
  plugins: [
    new ModuleFederationPlugin({
      name: "uniqueName", // 해당 앱의 유일한 이름. 중복된 이름이 있으면 안된다.
      filename: "remoteEntry.js", // 해당 앱을 다른 앱에서 사용하기 위한 정보가 담긴 Mnaifest 파일 이름 지정 (기본값 remoteEngry.js)
      exposes: {
        // key, value로 이루어진 Object로 외부에서 사용하기 위해 expose(노출)시킬 모듈들을 정의한다.
        "./App": "./src/App",
        "./Header": "./src/Header",
      },
      shared: {
        // 런타임에 앱간 공유하거나 공유받은 의존성 패키지를 정의한다.
        ...deps, // 이렇게 설정하면 package.json에 있는 dependencies를 모두 공유하거나 공유받겠다는 뜻
        react: {
          singleton: true,
        },
        "react-dom": {
          singleton: true,
        },
      },
    }),
  ],
};
```

expose에 정의된 모듈들은 모두 expose 가능하다.

```javascript
// 사용하는 쪽 webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: "app1",
      remotes: {
        uniqueName: "uniqueName@path/remoteEntry.js", // 노출한 모듈을 앱이 사용하기 위해 리모트 앱들을 정의한다. (`${name}@${path}` 형태)
        // key값은 별칭이다.
      },
      shared: { react: { singleton: true }, "react-dom": { singleton: true } },
    }),
  ],
};
```

## 더 깊게 들어가기
