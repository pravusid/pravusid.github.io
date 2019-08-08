---
layout: post
title: Node Version Manager (Node.js)
categories:
  - JavaScript
tags:
  - node-version-manager
  - nodejs
  - node
  - n
  - nvs
  - nvm
  - fnm
last_modified_at: 2019-08-08T22:22+09:00
comments: true
---

사실 `nodejs`를 사용하면서 버전에 엄청 민감한 상황이 발생한 적은 거의 없다.
그러나 특정 버전에서만 발생하는 오류를 확인/회피 하거나, docker까지 설정하지 않고 간편하게 배포 환경과 동일한 버전의 런타임 환경에서 개발하려면 버전 관리자가 필요하다.

여러 버전 관리자를 쓰면서 느꼈던 사항을 간단히 정리하였다.

## Version Manager

대부분의 버전 관리자가 node 경로참조를 위해 환경변수를 사용한다.

사용자가 node 버전을 지속적으로 변경하기 때문에 로그인 할 때 한 번만 읽는 shell 설정파일이 아니라,
세션 실행마다 읽어들이는 설정파일에 버전 관리자의 `PATH` 설정 스크립트를 불러오도록 되어있다.

GUI 환경에서 사용하는 터미널 에뮬레이터는 기본적으로 non-login shell을 실행시키므로
`bash`의 경우 `.bashrc`, `zsh`의 경우 `.zshenv` 같은 파일에 설정 스크립트를 연결한다.

## npm prefix

버전 관리자와 직접적인 관련이 있는 것은 아니지만 nodejs에서 `PREFIX` 개념은 통용되는 듯 하다.

nodejs global module 등의 설치 경로에 사용되는 것이 `prefix`이다.
여러 node 버전 매니저에서 `PREFIX` 환경변수로 경로를 조정한다.

> node prefix 확인: `npm config get prefix`

## n

<https://github.com/tj/n>

node계의 아이돌이**었던** tj가 개발한 버전 관리자이다.

- `$PREFIX` 환경변수 경로에 `n`이 설치된다
- `$PREFIX`를 설정하지 않았다면 기본 값은 `/usr/local`이다
- `PREFIX`, `N_PREFIX`를 `$HOME/n`으로 설정한다면 다른 버전 관리자처럼 특정 사용자 범위에서만 사용할 수 있다

기본적으로 `/usr/local/n/versions/` 경로에 nodejs 런타임 원본이 위치하고
`/usr/local/bin`, `/usr/local/lib` 등의 경로에 있는 nodejs 파일을 대체 한다. (삭제 후 복사 === 덮어쓰기)

이는 `PREFIX`가 변경되어도 동일한 방식으로 작동한다. (덮어쓰기)

실제로는 동시에 하나의 버전만 사용가능하므로 기본 `PREFIX`(`/usr/local`) 경로에서 시스템 nodejs 런타임으로 활용할 때 사용하는 것이 가장 적합해보인다.
시스템 nodejs 런타임으로 스크립트 실행 Cron Job을 실행하려면 `/usr/bin`, `/usr/local/bin` 경로에 위치해야 하기도 하고, 버전 변경이 자주 발생하지 않기 때문이다.

반면, 시스템 경로에 일반적인 방식으로 nodejs를 설치하면 버전관리 자체는 번거롭기 때문에 `n`의 가치가 없는 것은 아니다.

## nvs

<https://github.com/jasongin/nvs>

Microsoft 직원이 만든 버전 관리자이다.
그래서인지 VSCode debugger에서 지원한다. (VSCode는 사용자가 많은 `nvm`도 지원한다)

또한, 다른 버전 관리자가 제대로 지원하지 않는 Windows 환경도 지원하는 것으로 보인다. (Powershell을 사용해야 겠지만)

다른 버전 관리자와 마찬가지로 환경변수(의 PATH)를 사용하여 경로를 처리하는 것으로 보인다.

## nvm

<https://github.com/nvm-sh/nvm>

Github Star상으로 최고 인기 버전 관리자이다.

경로 처리는 환경변수를 사용하는 것으로 보인다.
여러 터미널을 동시에 실행 할 때 각 세션마다 다른 `node` 버전을 사용할 수 있다.

다만, symbolic link를 지원하지 않아서 시스템 런타임으로 사용하거나 VSCode 확장기능에서 node를 호출하는 경우 node 경로를 찾지 못하는 문제가 발생한다.

VSCode debugger에서는 nvs와 함께 지원하지만 `launch.json` 파일의 `runtimeVersion` 옵션에 버전을 명시적으로 입력해야 되어서 불편하다.

> symbolic link 관련 issue: <https://github.com/nvm-sh/nvm/issues/851>

Github Issue도 여러 번 등록되었는데, Maintainer들은 Symbolic 링크를 통해 기본 node 버전을 `PATH`에 고정적으로 등록하는 것이 nvm의 역할이 아닌 것으로 보는 듯 하다.

대충 issue를 살펴보니, symbolic 링크를 위해 alias를 사용하게 되는데, default alias는 사용하지 않는 사람도 있을 수 있고, current alias는 동시에 여러 세션이 열려있고 여러 버전이 실행되는 상황에서 명확한 상태를 갖지 않는 것으로 해석된다.

이와 관련하여 직접적으로 에디터/IDE 및 시스템 환경에서 `nvm`은 목적 적합하지 않고 `n`을 쓰라는 의견을 주기도 하였다.

## fnm

<https://github.com/Schniz/fnm>

최근에(2019년) 시작된 node 버전 관리자 프로젝트이다.

기존의 버전 관리자들이 shell script 혹은 JavaScript로 작성되었던 것에 비해 `fnm`은 코드에서 OCaml 비중이 70%를 넘는다.
실제로 사용해보니 `nvm` 보다 훨씬 빠르다.

사실, nvm을 사용하면서 가장 큰 문제로 느꼈던 점이 퍼포먼스이다.
평소에 `zsh`를 사용하면서 크게 느리다는 느낌이 없었으나 `nvm`을 사용한 이후 `PATH`를 설정하기 위한 `nvm.sh` 스크립트와 auto completion을 위한 스크립트를 실행하면서 shell 초기화 속도가 눈에 띄게 느려졌다.

`fnm`은 빠른데다 `alias` 경로가 symbolic link로 되어있으므로 `alias`를 `PATH`에 등록해서 편리하게 사용할 수 있다.

nvm 개발자들이 symbolic link 제공에 대한 부정적인 입장을 유지함으로써 (VSCode와 같은 에디터에서)발생하는 불편함이 `fnm`을 찾게 만든 원인이기도 하다.

`--multi` 옵션을 사용하면 `nvm`과 마찬가지로 shell 세션마다 다른 버전을 사용가능한 것으로 보이는데 `nvm` 메인테이너가 걱정했던 불일치가 발생할 수 있을지도 모르지만 해당 사항을 알고 있다면 약간 주의해서 사용하면 된다.

아직 기능이 조금 부족하기는 하지만 필요한 사항은 모두 갖춰져 있는 듯 하다.

## 맺음

아직 리눅스를 잘 알지 못해서 경로를 인식하지 못해 발생하는 오류로 인해 스트레스가 쌓여간다.
만병 통치약 같은 버전 관리자가 등장했으면 좋겠다.

> 틀린 내용을 발견하신다면 댓글에 남겨주세요
