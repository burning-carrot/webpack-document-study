# Configuration

### 개요

- 흔히 알고 있는 webpack파일은 webpack 설정을 만들어내는 js 파일이기 때문에 js 고유의 문법, require, 타 모듈 호출 등에 있어 매우 자유롭다
- 아래는 안티패턴
  - webpack cli에서 cli 인자에 직접 접근
  - 확실치 않은 값 export하기
  - 설정이 긴 webpack 스크립트를 한 파일에 작성하는 것
- js이외의 언어로도 설정이 가능하다.



### Exporting multiple configurations

- 단순히 js파일 하나에 정적인 webpack설정을 담는 것 이외에도 여러 방법이 있다.



#### 함수로 export

- 공통된 부분을 가지되 특정 환경마다 조금씩 다른 설정이 필요한 경우, 함수로 만들어서 변경이 필요한 부분만 파라미터로 받아 처리할 수 있다.



#### Promise로 export

- 함수로 export하는 방식과 맥락은 동일하지만 설정에 필요한 변수를 비동기적으로 받아야 할 때 유용하다.



#### 여러 설정 export  

```
module.exports = [
  {
    output: {
      filename: './dist-amd.js',
      libraryTarget: 'amd',
    },
    name: 'amd',
    entry: './app.js',
    mode: 'production',
  },
  {
    output: {
      filename: './dist-commonjs.js',
      libraryTarget: 'commonjs',
    },
    name: 'commonjs',
    entry: './app.js',
    mode: 'production',
  },
];
module.exports.parallelism = 1;
```

- 위의 설정처럼 한 모듈안에 배열의 형태로 여러 설정객체를 넣을 수 있다.
- 배열에 순회하면 각 객체에 맞게 모든 설정이 빌드된다.
- parallelism 옵션을 사용해서 동시에 병렬로 처리된 컴파일러 수를 정할 수 있다.