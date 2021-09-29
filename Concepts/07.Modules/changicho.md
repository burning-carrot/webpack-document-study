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

---

## CommonJS vs AMD vs RequireJS vs ES6 Modules

[link](https://medium.com/computed-comparisons/commonjs-vs-amd-vs-requirejs-vs-es6-modules-2e814b114a0b)

### CommonJS

```javascript
// require
var customerStore = require('store/customer'); // import module

// exports
exports = function(){
    return customers.get('store);
}
```

### NodeJS

CommonJS specification에 강하게 영향을받았다.

exports가 object인것이 다르다.

```javascript
// require
var customerStore = require('store/customer'); // import module

// exports
function customerStore(){
    return customers.get('store);
}
modules.exports = customerStore;
```

### Asynchronous Module Definition (AMD)

CommonJS이 아직 브라우저에 적합하지 않았을 때 만들어졌다.

이름에서 알 수 있듯이 비동기 모듈 로딩일 지원한다.

```javascript
define(["module1", ",module2"], function (module1, module2) {
  console.log(module1.setName());
});
```

함수는 모듈 로딩이 완료된 뒤에 실행된다.

### RequireJS

RequireJS는 AMD API를 상속한다.

일반 script태그에서 javascript 파일을 로딩한다.

### ECMAScript 6 modules (Native JavaScript)

ES6, ES2015에서 모듈 import, export를 지원했다. (동기 비동기를 모두)

```javascript
// export
export const sqrt = Math.sqrt;
export function square(x) {
  return x * x;
}
export function diag(x, y) {
  return sqrt(square(x) + square(y));
}

// import
import { square, diag } from "lib";

console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```
