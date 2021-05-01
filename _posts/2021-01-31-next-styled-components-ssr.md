---
title: NextJS에서 styled-components SSR 구현하기
author: MingtypE
date: 2021-01-31 00:00:00 +0900
categories: [Programming, Next]
tags: [next, react, styled-components, ssr]
---

# Intro

Next.js에서 styled-components를 사용할 때 **SSR**을 지원하기 위해 따로 설정이 필요하다. 이건 Next.js에서 예시를 제공하기 때문에 그냥 붙여넣어 쓰면 된다.

[참고 링크](https://github.com/vercel/next.js/tree/canary/examples/with-styled-components)

## Required

- babel-plugin-styled-components
- babel-plugin-inline-react-svg (svg 사용 시)

## Full Code

> `_document.tsx`와 `.babelrc` 파일에 아래 코드를 붙여 넣는다.

`pages/\_document.tsx`

```ts
import Document, { DocumentContext } from "next/document";
import { ServerStyleSheet } from "styled-components";

export default class MyDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const sheet = new ServerStyleSheet();
    const originalRenderPage = ctx.renderPage;

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App: any) => (props: any) =>
            sheet.collectStyles(<App {...props} />),
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        ),
      };
    } finally {
      sheet.seal();
    }
  }

  render() {
    return (
      <Html>
        <Head>/* meta, font등 설정 */</Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}
```

`.babelrc`을 루트에 생성한 후 아래 코드를 붙여넣기

```json
{
  "presets": ["next/babel"],
  "plugins": [
    [
      "babel-plugin-styled-components",
      { "ssr": true, "fileName": true, "displayName": true }
    ],
    "inline-react-svg" // svg 사용 시
  ]
}
```
