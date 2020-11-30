---
layout: post
title: 리눅스 gnome(그놈) 데스크톱 환경 다크모드 적용 오류
categories:
  - Linux
tags:
  - linux
  - gnome3
  - dark-theme
  - dark-mode
  - night-mode
last_modified_at: 2020-11-30T22:22+09:00
comments: true
---

어느 순간부터 그놈(gnome)3 데스크탑 환경에서 라이트모드-다크모드가 자동전환 되었다.

이는 `gnome-layout-switcher` -> `Settings` -> `Automatic dark theme` 옵션으로 작동한다.

> Automatic dark theme 옵션은 그놈셸 확장 `Night Theme Switcher` 기반이다.

이 옵션이 켜져있으면 다음 현상이 발생한다.

1. 낮 시간에 다크 테마를 적용할 수 없다(time source를 자동적용하는 경우)
2. 야간모드(night mode)가 활성화 되어있는 경우 다크모드가 반대로 적용될 수 있다
