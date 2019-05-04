---
layout: post
title: ESLint(TSLint)와 Prettier 함께 사용하기
categories:
  - JavaScript
tags:
  - eslint
  - tslint
  - prettier
last_modified_at: 2019-03-10T17:30+09:00
comments: true
---

코드 분석기인 ESLint와 formatter인 Prettier를 함께 사용하기 위해 필요한 사항을 알아보자

> TypeScript 도구인 TSLint에도 동일하게 적용할 수 있다

ESLint는 현재 가장 많은 사용자가 있는 JavaScript linter이다.

ESLint에도 formatting 관련 규칙들이 있지만, Prettier는 formatting에 특화되어 있다.

이 둘을 각각 설치해서 사용할 수도 있겠지만, 충돌 없이 깔끔하게 사용하기 위해서는 약간의 설정이 필요하다.

## Prettier

### Prettier 하위 프로젝트

Prettier와 ESLint를 함께 사용하기 위한 프로젝트들이다

#### [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)

Prettier를 ESLint 플러그인으로 추가한다.
즉, Prettier에서 인식하는 코드상의 포맷 오류를 ESLint 오류로 출력해준다.

#### [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

ESLint의 formatting관련 설정 중 Prettier와 충돌하는 부분을 비활성화 한다.

#### [prettier-eslint](https://github.com/prettier/prettier-eslint)

위의 둘과 다른 성격의 프로젝트이다.

ESLint와 Prettier를 연동해서 사용하는 것이 아니라, 순차적으로 적용한다.

Prettier로 코드를 Formatting한 뒤(`prettier`) ESLint로 수정(`eslint --fix`)한 것과 같은 결과물을 출력한다.

> `Code` -> `prettier` -> `eslint --fix` -> `Formatted Code`

결국 텍스트 에디터나 CLI상에서는 `.eslintrc`에 명시한 것들만 포맷 오류로 출력된다.

### `.prettierrc`

prettier의 포맷을 적용하기 위한 설정파일이다.

기본적으로 프로젝트 root에 위치한 `.prettierrc` 설정을 적용하고, 설정이 없는 경우 prettier의 기본값이 적용된다.

설정은 다음 페이지에서 확인할 수 있다: <https://prettier.io/docs/en/configuration.html>

`.prettierrc` 설정 예시

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "singleQuote": true,
  "trailingComma": "all",
  "bracketSpacing": true,
  "semi": true,
  "useTabs": false,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

## ESLint + Prettier

아래의 두 방법 중 하나를 선택하면 된다.

JavaScript & TypeScript에서 인기가 높은 텍스트 에디터인 **VSCode**를 기준으로 설명한다.

VSCode에 적용하기 위해서 우선 확장 기능을 설치해야 한다

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

ESLint 확장기능은 eslint dependency 미포함, Prettier 확장기능은 prettier 및 pretter-es/tslint dependencies 포함이지만,
설정파일(config) 및 플러그인(plugin)과 함께 사용하기 위해서 dev dependencies로 local node_modules로 관리하는 것을 추천한다.

아래의 ESLint + Prettier는 eslint 설정이 완료된 상황임을 가정한다.

만약 eslint가 준비되지 않았다면 다음 페이지에서 eslint 초기설정을 확인할 수 있다.

<https://eslint.org/docs/user-guide/getting-started>

### 1. ESLint + eslint-plugin-prettier + eslint-config-prettier

공식 문서: <https://prettier.io/docs/en/eslint.html>

**plugin**이나 **config** 중 하나만을 사용할 수도 있다

- plugin만 사용: 포맷 관련 오류가 두 번 출력된다(eslint + prettier)
- config만 사용: eslint에서 포맷 관련 오류가 출력되지 않는다

plugin 사용만으로는 eslint formatting rules와 prettier rules가 충돌하므로, eslint-config-prettier를 함께 사용한다
(공식문서에서도 둘을 함께 사용하기를 권장한다)

따라서 둘을 함께 사용하는 경우를 서술함

DevDependencies 추가: `npm i -D prettier eslint-plugin-prettier eslint-config-prettier`

`.eslintrc.json` 설정에 추가

```json
{
  "extends": ["plugin:prettier/recommended"]
}
```

기존에 다른 eslint rule (airbnb, standard, google ...)을 사용하고 있다면 함께 확장(extends)하면 된다.

```json
{
  "extends": ["airbnb-base", "plugin:prettier/recommended"]
}
```

### 2. ESLint + prettier-eslint

`prettier-eslint`를 활성화 하기 위해서 VSCode 설정에 다음을 추가한다

```json
{
  "javascript.format.enable": false,
  "prettier.eslintIntegration": true
}
```

VSCode에서 사용할 때 별도의 Prettier 관련 dev dependencies를 설치하지 않아도 된다 (Prettier 확장에 포함되어 있음)

만약 VSCode 확장기능을 쓰지 않고 `prettier-eslint`를 적용하려면 `prettier`, `prettier-eslint`를 dev dependencies로 설치하고,
[prettier-eslint-cli](https://github.com/prettier/prettier-eslint-cli)를 사용한다.

## TSLint + Prettier 적용

### 1. TSLint + tslint-plugin-prettier + tslint-config-prettier

ESLint + Prettier와 거의 동일하지만 `tslint-plugin-prettier`, `tslint-config-prettier` 설정이 약간 다르다.

해당 프로젝트의 github에 있는 README에서 설정방법을 확인할 수 있다

- plugin: <https://github.com/prettier/tslint-plugin-prettier>
- config: <https://github.com/prettier/tslint-config-prettier>

### 2. TSLint + prettier-tslint

`prettier-tslint`를 사용하려는 경우 VSCode 설정에 다음을 추가한다

```json
{
  "typescript.format.enable": false,
  "prettier.tslintIntegration": true
}
```

VSCode와 별도로 사용하려면 `prettier`, `prettier-tslint`를 DevDependencies로 설치하면 된다.

`prettier-tslint`에는 cli가 포함되어 있다: <https://github.com/azz/prettier-tslint>

## 맺음

포맷과 관련된 코딩 컨벤션을 모두 Linter에서 관리할 수 있다.

다만 Prettier를 사용한다면 Prettier의 Formatting 기능이 조금 더 강력하고(진짜인가??),
코드관련 분석과 포맷팅 관련 분석을 분리하여 관리한다는 것에 의미를 두고 두 도구를 함께 사용하는 것 같다.

여러 사람들이 사용한다기에 따라서 linter와 formatter를 섞어서 사용해보고 있으나, 이전과 차이를 체감하기는 어려운 듯 하다.

> 틀린 부분을 발견하신다면 댓글에 남겨주세요
