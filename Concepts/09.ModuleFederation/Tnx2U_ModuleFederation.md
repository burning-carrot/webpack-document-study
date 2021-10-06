## ModuleFederation 

### 개요

- 하나의 앱이 다르 빌드에 있는 코드를 동적으로 실행시키는 기술. 한 빌드가 remote 앱이 되어 다른 빌드들을 동적으로 불러와 사용한다.
- Micro-frontend의 근간이 되는 기술이다.

<br/>

### low-level concept

- 모듈을 로컬모듈과 원격 모듈로 구분한다.
  - 로컬 모듈 : 현재 빌드 내부에 포함되어있는 모듈
  - 원격 모듈 : 현재 빌드에 없고 런타임 중에 원격 컨테이너(remote 앱)에서 가져와서 사용해야 하는 모듈
- 원격 모듈을 가져오는 것은 런타임에 비동기로 동작한다.
  - 코드상에서 import와 같은 로드 작업 이후에 실행된다.

<br/>

### high-level concept

- 각 빌드는 서로 다른 빌드를 컨테이너로써 사용하거나 본인이 컨테이너가 된다.
- 즉, 개요에서 설명한 remote앱이 특정 한 빌드만 되는 것이 아니라, 여러개의 빌드가 서로에게 remote앱으로 동작할 수 있다.

<br/>

### Building Blocks

- module federation 동작에 영향을 미치는 플러그인들이 있다.
  - ContainerPlugin
  - ContainerReferencePlugin
  - ModuleFederationPlugin

<br/>

### 사용예시

- 각 페이지를 SPA로 만들어서 빌드하고 서로를 모듈로써 호출하는 구조.

  - 각 페이지별로 빌드 및 관리가 가능
- Components library as container
- Dynamic Remote Containers
  - init, get과 같은 컨테이너 인터페이스를 만들어서 비동기로 동작하게 만들어 여러 컨테이너가 이 컨테이너를 공유해서 사용하도록 만드는 구조인듯

<br/>

### Dynamic Public Path

- 호스트가 원격 모듈의 메소드를 사용할때 동적으로 public path를 지정한다.
- 호스트 도메인 하위 경로에 독립적으로 다른 빌드를 배포하여 사용할 수 있다.

<br/>

### 트러블슈팅

- 에러별 예시 참고
- https://webpack.kr/concepts/module-federation/#troubleshooting

<br/>

<br/>

### 참고 사이트

- https://feed.hoondev.com/webpack/micro-frontends%EB%A5%BC-%EC%9C%84%ED%95%9C-webpack-5-module-federation/
- https://yceffort.kr/2020/11/webpack-module-federation-example

