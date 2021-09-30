## Module Resolution

### resolver

- 웹팩의 resolver는 모듈을 가져오는 코드구문을 인식해 해당 코드 혹은 외부 라이브러리를 가져오는 역할을 한다.
- 일반적으로 웹팩에서는 enhanced-resolve를 사용한다.



### 경로 처리방법

- 절대경로 : 있는 그대로 사용

- 상대경로 : 해당 상대경로에 해당 파일의 경로(컨텍스트 디렉토리)를 추가하여 절대경로를 생성해서 사용한다.

- Module paths

  - ```javascript
    module.exports = {
      //...
      resolve: {
        modules: ['node_modules'],
      },
    };
    ```

  - 위와 같이 웹팩설정에서 resolve 설정에 추가한 모듈들을 가져온다(resolve.module)

  - 만약 package.json이 있다면 resolve에서 아래와 같이 지정할 수 있다.

    - ```javascript
      module.exports = {
        //...
        resolve: {
          exportsFields: ['exports', 'myCompanyExports'],
        },
      };
      ```

  - 위의 과정을 통해 확인된 경로를 리졸버가 해당 경로가 파일인지 디렉토리인지 체크한다.

  - 경로가 파일인 경우 : 경로에 파일 확장자가 있으면 바로 번들로 사용하고, 없으면 resolve.extestions 옵션으로 확인한다.

  - 경로가 디렉토리인 경우 : 해당 디렉토리에 package.json이 있는지 체크하여 resolve.mainFields에 지정된 필드 순서로 조회한다. 이에 해당되지 않으면, resolve.mainFields에 지정된 파일 이름 순서로 검색하여 일치하는게 있는지 찾는다.

  - 확장자 체크는 동일하다.



### 캐싱

- 캐싱으로 동일 파일에 대한 처리를 더 빠르게 할 수 있다.
- watch모드를 키면 변경된 사항만 삭제하고, 끄면 모든 캐시를 다 삭제한다.