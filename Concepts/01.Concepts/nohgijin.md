![](https://i.imgur.com/PvIiLa0.png)


## Concepts
- 여러개의 파일을 하나의 파일로 합쳐주는 번들러
- webpack이 애플리케이션을 처리할 때, 내부적으로 프로젝트에 필요한 모든 모듈을 매핑
- 하나 이상의 번들을 생성하는 디펜던시 그래프 제작

## Entry
- webpack은 entry point에 의존하는 모듈과 라이브러리를 찾아냄
- 기본값은 `./src/index.js`

## Output
- 생성된 번들을 내보낼 위치와 이름을 지정하는 요소
- 기본값은 `./dist/main.js`, 기타 파일의 경우 `./dist` 폴더로 설정
```
output: {
    path: path.resolve(__dirname,'dist'), //__dirname:webpack.config.js 의 위치
    filename:'my-first-webpack.bundle.js'
}
```

## Loader
- 파일 단위로 처리
- 로더를 사용시 webpack이 다른 유형의 파일을 처리, 유효한 모듈로 변환하여 애플리케이션에서 사용
- typescript를 javascript로, 이미지를 data URL 형식의 문자열로 변환해준다.
- 로더로 인하여 웹팩은 모든 파일(js, stylesheet, img, font)을 모듈로 바라보므로, import 구문을 사용하여 자바스크립트 코드 안으로 가져올 수 있음
- `test` 속성(변환이 필요한 파일을 찾음)과 `use` 속성(변환을 수행하는데 사용된 로더)을 가짐
`rules: [{ test: /\.txt$/, use: 'raw-loader' }]`

### css-loader
- css 파일을 자바스크립트에서 불러와 사용하려면 모듈로 변환하는 작업을 함
- webpack이 css 파일을 찾을 경우 css-loader로 처리함
- 이로 인해 css 코드가 자바스크립트 코드로 변환됨

### style-loader
- 자바스크립트로 변경된 코드를 동적으로 돔에 추가함

## Plugins
- 번들된 결과물을 처리
- 번들된 자바스크립트를 난독화, 특정 텍스트를 추출하는 용도로 사용
- 번들을 최적화, assets 관리, 환경변수 주입
- 대부분 new 연산자로 호출하여 플러그인의 인스턴스를 만들어 사용
`plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })]`

## Mode
- development, production, none으로 설정 시 환경별 최적화를 활성화 가능
```
module.exports = {
  mode: 'production',
};
```
```
module.exports = {
  mode: 'development',
};
```
## Browser Compatibility
- ES5가 호환되는 모든 브라우저 지원
- import() 및 Promise를 요구하므로 폴리필 로드 필요

## Code Spliting
- bundler를 통해서 합해진 많은 코드들을 쪼개는 과정

## Lazy Loading
- 쪼개진 코드를 필요할 때 마다 로딩하는 것
