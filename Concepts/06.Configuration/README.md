# 2021.09.15

다음 스터디는 9월 30일 (추석은 쉬기)

## 의문점

Configuration에서

- 결정되지 않은 값을 내보내기 (webpack을 두 번 호출하면 동일한 출력 파일이 생성됩니다)
    - webpack 여러번 쳐도 같은 출력이 나와야 된다는 말인듯하네여

Plugins

- 플러그인 new로 생성하는거면 그냥 객체로 만들어도 잘 되는지?

## 플러그인 예시

```javascript
// webpack.config.js

const webpack = require("webpack")
const childProcess = require("child_process")

const removeNewLine = buffer => {
  return buffer.toString().replace("\n", "")
}
module.exports = {
  plugins: [
    new webpack.BannerPlugin({
      banner: `
        Build Date :: ${new Date().toLocaleString()}
        Commit Version :: ${removeNewLine(
          childProcess.execSync("git rev-parse --short HEAD")
        )}
        Auth.name :: ${removeNewLine(
          childProcess.execSync("git config user.name")
        )}
        Auth.email :: ${removeNewLine(
          childProcess.execSync("git config user.email")
        )}
  `,
    }),
  ],
}
```

```javascript
/*!
 *
 *         Build Date :: 2021. 1. 2. 오후 10:54:53
 *         Commit Version :: d131165
 *         Auth.name :: cckn
 *         Auth.email :: cckn.dev@gmail.com
 *
 */
```

https://github.com/webpack-contrib/webpack-bundle-analyzer

## 다음 스터디

9월 30일 오후 8시

범위
- Modules
- Module Resolution
