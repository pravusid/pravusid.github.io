---
layout: post
title: pamac-cli 업데이트 도중 충돌 오류
categories:
  - Linux
tags:
  - linux
  - arch-linux
  - pamac
last_modified_at: 2021-01-03T20:30+09:00
comments: true
---

> pamac-cli와 pamac-gtk가 충돌합니다(pamac)

`aur/pamac-cli` 패키지 업데이트 중 위 오류가 발생하는 경우 관련 의존성을 모두 다시 설치하면 된다

```sh
sudo pacman -Syuu
sudo pacman -S pamac-gtk-dev pamac-cli-dev pamac-common-dev
```
