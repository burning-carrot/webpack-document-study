# Modules

Node.js는 거의 시작부터 모듈형 프로그래밍을 지원했으나, 웹에서는 모듈의 지원이 느리게 정착해왔다.

Webpack은 프로젝트의 모든 파일에 모듈의 개념을 사용한다.

## What is a webpack Module

Node.js 모듈과 달리 webpack 모듈은 다양한 방식으로 의존성을 표현할 수 있다.

- ES2015 `import`문
- CommonJS `require()`문
- AMD `define`, `require`문
- CSS / SASS / LESS파일 내의 `@import`문
- 스타일 시트 `url(...)`의 이미지 URL 또는 HTML `img src`문

## Supported Module Types

Webpack은 기본적으로 다음 유형의 모듈을 지원한다.

- ECMAScript 모듈
- CommonJS 모듈
- AMD 모듈
- Assets
- WebAssembly 모듈

webpack은 여러 언어로 작성된 모듈과 로더를 통한 다양한 전처리기를 지원한다.

로더는 webpack에서 네이티브가 아닌 모듈을 어떻게 처리하고 이러한 의존성을 번들에 포함할지 정의한다.

다음과 같이 널리 사용되는 다양한 언어와 언어 프로세서를 위한 로더를 제작했다.

- CoffeeScript
- TypeScript
- ESNext (Babel)
- SASS
- Less
- Stylus
- Elm

## Further Reading

[JavaScript Module Systems Showdown](https://auth0.com/blog/javascript-module-systems-showdown/)
