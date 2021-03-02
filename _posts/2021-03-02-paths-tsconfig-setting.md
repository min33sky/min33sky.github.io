---
title: tsconfig에서 paths 설정 시 eslint 에러 해결하기
author: MingtypE
date: 2021-03-02 00:00:00 +0900
categories: [Programming, React]
tags: [react, typescript, eslint]
---

# Intro

모듈을 불러올 때 **'../../../../Component'** 이런 식으로 상대 경로가 길어지는 경우가 많다. 길어진다고 성능에 지장을 주는건 아니지만 가독성이 떨어지므로 `tsconfig`의 `paths` 속성으로 별칭을 지어주면 절대경로처럼 사용할 수 있다.

### tsconfig.json 예시

```json
  "paths": {
      "@hooks/*": ["hooks/*"],
      "@components/*": ["components/*"],
      "@pages/*": ["pages/*"],
    }
```

### 모듈을 불러올 때

```ts
import Login from "@page/Login";
```

## Error

`eslint`를 사용한다면 위의 경우에서 **Unable to resolve path to module '@page/Login' eslint(import/no-unresolved)** 에러가 발생할 것이다.
다음 과정을 통해 에러를 해결해 보겠다.

- 모듈 설치

[eslint-import-resolver-typescript](https://www.npmjs.com/package/eslint-import-resolver-typescript) 모듈을 설치한다.

- .eslintrc.json에 다음 코드를 추가한다.

```json
 "settings": {
    "import/parsers": {
      "@typescript-eslint/parser": [".ts", ".tsx"]
    },
    "import/resolver": {
      "typescript": {}
    }
 }
```

- 에러가 안사라졌다면 vscode 재실행
