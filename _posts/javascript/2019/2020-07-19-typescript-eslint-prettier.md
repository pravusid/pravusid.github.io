---
layout: post
title: TypeScript ESLint + Prettier 함께 사용하기(w/ VSCode)
categories:
  - TypeScript
tags:
  - eslint
  - typescript
  - prettier
  - typescript-eslint
  - vscode
last_modified_at: 2020-07-19T21:10+09:00
comments: true
---

TSLint 프로젝트가 deprecated 상태가 되고 ESLint에 통합되었다.
<https://github.com/palantir/tslint/issues/4534>

2020년 까지만 TSLint를 지원하기 때문에 신규 프로젝트라면 typescript-eslint를 사용해야 한다.

이미 발표이후 typescript-eslint 사용법에 대한 게시물이 많이 올라왔지만,
<https://pravusid.kr/javascript/2019/03/10/eslint-prettier.html> 게시물에 대한 A/S 차원에서 뒤늦게 작성해본다.

## typescript-eslint

[typescript-eslint](https://github.com/typescript-eslint/typescript-eslint) 프로젝트는 ESLint의 TypeScript 지원을 위한 패키지의 MonoRepository이다.

주요 패키지의 기능에 대한 설명은 다음에서 확인할 수 있다.

<https://github.com/typescript-eslint/typescript-eslint#packages-included-in-this-project>

- `@typescript-eslint/typescript-estree`: ESTree(자바스크립트 AST(추상구문트리) 스펙)호환 AST를 생성하는 타입스크립트 파서
- `@typescript-eslint/parser`: `typescript-estree`를 활용한 타입스크립트용 ESLint 파서(ESLint 기본 파서인 `espree`를 대신함)
- `@typescript-eslint/eslint-plugin`: TypeScript관련 linting 규칙을 처리하는 플러그인

### typescript-eslint 적용

typescript, eslint, typescript-eslint 관련 패키지가 필요하다

```sh
npm i --save-dev typescript eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

`.eslintrc.json` 파일을 설정한다

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "env": {
    "node": true
  },
  "extends": ["plugin:@typescript-eslint/recommended"]
}
```

`"extends": ["plugin:@typescript-eslint/recommended"]` 설정을 통해 `@typescript-eslint/eslint-plugin` 적용과 동시에 `recommened` 규칙을 확장한다

`recommened` 규칙 세부목록은 다음 페이지에서 확인할 수 있다.

<https://github.com/typescript-eslint/typescript-eslint/tree/master/packages/eslint-plugin#supported-rules>

typescript-eslint는 구문분석을 하면서 AST를 생성하므로 상당히 강력한 규칙들을 적용 가능하다.

규칙중 `requires type information` 유형에 해당하는 규칙은 컴파일러 분석에서 조금 아쉬웠던 부분을 긁어주는 규칙이다.
(처리 속도 문제로 `recommened` 규칙에서는 제외되어 있다.)

### typescript-eslint 실행

```sh
# 기본적으로 js 확장자만 검사하므로
npx eslint --ext .js,.ts src
# 또는
npx eslint src/**/*
```

VSCode를 사용하고 있다면

[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)확장을 설치하고,

다음 설정을 적용한다

```json
{
  "eslint.validate": [
    { "language": "typescript", "autoFix": true },
    { "language": "typescriptreact", "autoFix": true }
  ]
}
```

## typescript-eslint + prettier

JavaScript와 마찬가지로 typescript-eslint와 prettier를 함께 사용할 수 있다.

> prettier와 eslint의 관계에 대해서는 다음 포스트를 참조: <https://pravusid.kr/javascript/2019/03/10/eslint-prettier.html>

### typescript-eslint + prettier 적용

앞의 `typescript-eslint` 적용 단계를 완료했다고 가정한다.

```sh
npm i --save-dev prettier eslint-plugin-prettier eslint-config-prettier
```

`.eslintrc.json` 설정에 다음을 추가한다

```json
{
  "extends": ["plugin:prettier/recommended", "prettier/@typescript-eslint"]
}
```

- `plugin:prettier/recommended`: eslint-plugin-prettier + eslint-config-prettier 동시 적용
- `prettier/@typescript-eslint`: prettier 규칙과 충돌하는 @typescript-eslint/eslint-plugin 규칙 비활성화

**최종적**으로 `.eslintrc.json` 설정은 다음과 같다.

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "env": {
    "node": true
  },
  "extends": ["plugin:@typescript-eslint/recommended", "plugin:prettier/recommended", "prettier/@typescript-eslint"]
}
```

> prettier 규칙은 다음페이지를 참고하여 생성할 수 있다: <https://prettier.io/docs/en/configuration.html>

VSCode를 사용하고 있다면(ESLint 확장은 `typescript-eslint` 적용 단계에서 설치했다고 가정)

[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 확장을 설치하고 다음 설정을 적용한다

```json
{
  "javascript.format.enable": false,
  "typescript.format.enable": false
}
```

> 잘못된 내용을 발견하신다면 댓글로 남겨주세요
