---
layout: post
title: Linux gnome desktop 환경에서 Dash to dock 아이콘 중복 해결
categories:
  - Tools
tags:
  - linux
  - ghome
  - dash-to-dock
  - icon
last_modified_at: 2020-01-14T22:22+09:00
comments: true
---

리눅스 Gnome 데스크톱 환경을 사용하는 상당수의 배포판은 Dash to dock을 기본 채용하고 있다.

Dash to dock에 자주 사용하는 애플리케이션을 즐겨찾기로 등록하여 사용할 수 있는데,
간혹 즐겨찾기로 등록되어 있는 아이콘을 클릭하면 실행한 앱의 아이콘이 추가적으로 생기는 현상을 발견할 수 있다.

이는 런처에 적절한 `StartupWMClass`가 지정되지 않아서이다.

## 해결

우선 해당 애플리케이션의 `StartupWMClass`를 알아내야 한다.

1. 아이콘 중복이 발생하는 애플리케이션을 실행하고
2. 터미널에서 `xprop WM_CLASS` 명령을 실행하면 커서가 십자로 바뀌는데, 1에서 실행한 애플리케이션을 클릭한다.
3. 터미널에 출력되는 `WM_CLASS`를 확인한다.

다음으로 애플리케이션 런처 파일을 찾는다. 다음의 경로에 존재할 수 있다.

- `/usr/share/applications`
- `~/.local/share/applications`

위의 경로에서 `애플리케이션이름.desktop` 파일을 찾아서 에디터로 수정한다.

`애플리케이션이름.desktop` 본문에 다음내용을 추가한다

`StartupWMClass=<앞에서 찾은 WM_CLASS 값>`
