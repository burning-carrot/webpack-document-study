# Module Resolution

(모듈 해석)

모듈은 다음과 같이 다른 모듈을 의존하며 필요로 할 수 있다. (의존성)

```javascript
import foo from "path/to/module";
// 또는
require("path/to/module");
```

## Resolving rules in webpack

`enhanced-resolve`를 사용해 webpack은 세 가지 종류의 파일 경로를 확인할 수 있다.

### Absolute paths

```javascript
import "/home/me/file";

import "C:\\Users\\me\\file";
```

이미 파일에 대한 절대 경로가 있으므로 추가 분석이 필요하지 않다.

### Relative paths

```javascript
import "../src/file1";
import "./file2";
```

이 경우 import 또는 require가 발생하는 리소스 파일의 경로를 현재 컨텍스트 경로로 간주한다. (현재 파일 기준)

import/require에 지정된 상대 경로는 이 컨텍스트 경로에 결합된다.

이후 모듈에 대한 절대 경로를 생성한다.

### Module paths

```javascript
import "module";
import "module/lib/file";
```

모듈은 resolve.modules에 지정된 모든 디렉터리 내에서 검색할 수 있다.

resolve.alias 구성 옵션을 사용하여 별칭을 만들어 원래 모듈 경로를 대체 경로로 바꿀 수 있다.

```javascript
// webpack.config.js
module.exports = {
  //...
  resolve: {
    modules: ["node_modules"],
    alias: {
      Utilities: path.resolve(__dirname, "src/utilities/"),
      Templates: path.resolve(__dirname, "src/templates/"),
    },
  },
};
```

- 패키지에 package.json파일이 포함된 경우 resolve.exportsFields 구성 옵션에 지정된 필드를 순서대로 조회한다.
- package.json의 첫 번째 필드는 패키지 내보내기 가이드라인에 따라 패키지에서 사용 가능한 내보내기를 결정한다.

위의 규칙에 따라 경로가 확인되면 리졸버는 경로가 파일 또는 디렉터리를 가리키는지 확인한다.

#### File check

경로가 파일을 가리키는 경우 다음을 수행한다.

- 경로에 파일 확장자가 있으면 파일이 바로 번들로 제공됩니다.
- 그렇지 않으면 파일 확장자를 resolve.extensions 옵션을 사용하여 확인한다.. (예시: .js, .jsx)

#### Folder check

경로가 폴더를 가리키는 경우 올바른 확장자를 가진 올바른 파일을 찾기 위해 다음 단계를 수행한다.

- 폴더에 package.json 파일이 포함되어 있으면 resolve.mainFields 구성 옵션에 지정된 필드가 순서대로 조회되고 package.json의 첫 번째 필드가 파일 경로를 결정한다.
- package.json이 없거나 resolve.mainFields가 유효한 경로를 반환하지 않는 경우
  - resolve.mainFields 구성 옵션에 지정된 파일 이름을 순서대로 검색하여 imported/required 된 디렉터리에 일치하는 파일 이름이 있는지 확인한다.
  - 그런 다음 resolve.extensions 옵션을 사용하여 비슷한 방식으로 파일 확장자를 확인합니다.

Webpack은 빌드 대상에 따라 이러한 옵션에 대해 합리적인 기본값을 제공한다.

## Resolving Loaders

파일 분석에 명시된 것과 동일한 규칙을 따른다.

그러나 resolveLoader 구성 옵션을 사용하여 로더에 대한 별도의 분석 규칙을 가질 수 있다.

## Caching

모든 파일 시스템 액세스가 캐시되므로 동일한 파일에 대한 여러 병렬 또는 직렬 요청에 더 빠르게 응답할 수 있다.

watch 모드에서는 수정된 파일만 캐시에서 제거된다.

watch 모드가 꺼져 있으면 모든 컴파일 전에 캐시가 제거된다.

```javascript
// webpack.config.js
module.exports = {
  //...
  watch: true,
};

// use watchOptions
module.exports = {
  //...
  watchOptions: {
    aggregateTimeout: 200,
    poll: 1000,
  },
};
```

위에서 언급한 구성 옵션에 대한 자세한 내용은 Resolve API에서 확인 가능하다.
