---
title: eslint에 prettier 그리고 typescript 설정하기
author: MingtypE
date: 2021-01-29 00:00:00 +0900
categories: [Programming, React]
tags: [react, eslint, typescript, prettier, setting]
---

## Intro

eslint와 prettier를 같이 사용할 때 충돌이 발생하는 경우가 많다.
아래의 설정을 통해서 이 문제를 해결해 보겠다. typescript는 덤이다.

## Install

```
npm i -D eslint eslint-config-prettier eslint-plugin-prettier
```

## Setting

1. `npx eslint --init` 실행 후 import, react, typescript, airbnb 선택

2. `.eslintrc.js`에 아래 코드를 추가

```js
extends: [
    'prettier/@typescript-eslint',
    'plugin:prettier/recommended',
  ],
```

3. `.prettierc`을 생성하여 원하는 prettier 설정을 추가하기

## Full Code

```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    "plugin:react/recommended",
    "airbnb",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12,
    sourceType: "module",
  },
  plugins: ["react", "@typescript-eslint"],
  rules: {
    "prettier/prettier": [
      "error",
      {
        singleQuote: true,
        tabWidth: 2,
        endOfLine: "auto",
      },
    ],
    "react/jsx-filename-extension": [
      1,
      { extensions: [".js", ".jsx", ".ts", ".tsx"] },
    ],
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off",
    "jsx-a11y/anchor-is-valid": "off",
    "react/jsx-props-no-spreading": "off",
    "import/no-extraneous-dependencies": "off",
    "jsx-a11y/label-has-associated-control": "off",
    "import/extensions": [
      "error",
      "ignorePackages",
      { js: "never", jsx: "never", ts: "never", tsx: "never", json: "never" },
    ],
  },
  settings: {
    "import/resolver": { node: { extensions: [".js", ".jsx", ".ts", ".tsx"] } },
  },
};
```
