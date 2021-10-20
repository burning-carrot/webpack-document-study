# The Manifest

webpack으로 빌드된 일반적인 애플리케이션 또는 사이트의 코드 유형은 주로 다음 3가지로 나눌 수 있다.

1. 개발자 또는 개발팀이 작성한 소스 코드
2. 소스 코드가 의존하는 써드 파티 라이브러리 혹은 "공급 업체" 코드
3. 모든 모듈의 상호 작용을 수행하는 webpack 런타임 및 매니페스트.

이중 3번째인 런타임과 특히 매니페스트에 대한 문서이다.

## Runtime

런타임은 기본적으로 webpack이 브라우저에서 실행되는 동안 모듈화된 애플리케이션을 연결하는 데 필요한 모든 코드이다.

이는 manifest 데이터 또한 마찬가지이다.

모듈이 상호 작용할 때 모듈을 연결하는 데 필요한 로딩 및 해석 로직이 포함되어있다.

(라우저에 이미 로드된 모듈 연결과 그렇지 않은 모듈을 지연 로드하는 로직)

## Manifest

> 명백한, 뚜렷한. (명사) 화물 목록

애플리케이션이 `index.html` 파일 형식으로 브라우저에 도달하면,
애플리케이션에 필요한 일부 번들 및 기타 다양한 애셋을 로드하고 연결해야 한다.

프로젝트 루트 디렉토리 (`/src`) 이제 번들이 되고 압축되며
지연 로딩을 위해 webpack의 최적화를 통해 더 작은 청크로 분할될 수 있다.

webpack은 필요한 모든 모듈 간의 상호 작용을 매니페스트 데이터로 관리한다.

컴파일러가 애플리케이션에 입력, 해석 및 매핑 할 때 모든 모듈에 대한 자세한 메모를 유지한다.

이 데이터 모음(자세한 메모)을 "매니페스트"라고 한다.

매니페스트는 모듈이 번들링 되고 브라우저에 제공되면 런타임에서 모듈을 해석하고 로드하는데 사용한다.

선택한 모듈 구문(Module Methods)에 관계없이 import 또는 require문은 이제 모듈 식별자를 가리키는 `__webpack_require__` 메소드가 된다.

(Module Methods : import, require ...)

매니페스트의 데이터를 사용하여 런타임은 식별자 뒤에 있는 모듈을 검색할 위치를 찾을 수 있다.

## The Problem

런타임은 매니페스트를 활용하여 작업을 수행하며 애플리케이션이 브라우저에 도달하면 기본적으로 잘 동작한다.

그러나 브라우저 캐싱을 활용하여 프로젝트의 성능을 향상해야할 경우 해당 프로세스는 이해해야 할 중요한 사항이 된다.

번들 파일 이름 내에서 콘텐츠 해시를 사용하면 파일 콘텐츠가 변경된 시기를 브라우저에 표시하여 캐시를 무효화 할 수 있다.

이 중 특정 해시는 내용이 분명히 변경되지 않더라도 변경된다. 이는 모든 빌드를 변경하는 런타임 및 매니페스트의 주입으로 인해서 발생한다.

## Further Reading

매니페스트를 추출하는 방법 :

- [Output management 가이드의 매니페스트 섹션](https://webpack.kr/guides/output-management/#the-manifest)

캐싱의 복잡성 :

- [Separating a Manifest](https://survivejs.com/webpack/optimizing/separating-manifest/)
- [Predictable Long Term Caching with webpack](https://medium.com/webpack/predictable-long-term-caching-with-webpack-d3eee1d3fa31)
- [Caching](https://webpack.kr/guides/caching/)
