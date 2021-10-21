## Targets

- 웹팩을 사용하는 JS는 서버와 클라이언트 모두 작성이 가능하기 때문에, target 속성으로 배포환경을 지정할 수 있다.

```jsx
module.exports = {
  target: 'node',
};
```

- targe





### 사내코드 예시

- electron 네이티브 어플리케이션이다
- base, electron, main, preload, renderer의 4개의 주요 웹팩js파일이 존재 각 파일의 모듈을 webpack-merge를 사용해서 base에 합친다.
- 각 파일에서는 target으로 다음을 사용한다.
  - electron-main
  - electron-renderer
  - electron-preload
- 일렉트론 구조상 노드서버와 react를 동시에 돌리며 통신하는 구조이기 때문에, 각 서버에 대해서 설정파일이 별도로 필요하기 때문인듯.



## The Mainfest

- 웹팩이 필요한 모든 모듈 간의 상호작용을 관리할때 메니페스트 데이터로 관리한다.
- 매니페스트란, 컴파일러가 애플리케이션을 입력, 해석, 매핑할때 모든 모듈에 대해 기록한 메타데이터이다. 이 메타데이터들(매니페스트)는 브라우저에 제공된 뒤, 런타임에서 "**webpack_require**"를 통해 모듈을 해석하고 로드하는데 사용된다.
- 캐싱을 위해선 메니페스트 동작구조에 대해 알 필요가 있다.
- 메니페스트 데이터 관리 플러그인
  - https://github.com/shellscape/webpack-manifest-plugin

