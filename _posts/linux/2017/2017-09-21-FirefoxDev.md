---
layout: post
title: 우분투에서 Firefox Developer Edition 설치
categories:
  - Linux
tags:
  - firefox-aurora
  - ubuntu
last_modified_at: 2017-09-21T16:34+09:00
comments: true
---

## 파이어폭스 Developer Edition

### 파이어폭스 Edition

파이어폭스에는 여러 에디션이 있었는데 정리과정을 겪었고, 현재는 크게 세 개가 남아있는듯 합니다.

1. Stable : 정식버전
1. Devloper(Aurora) : 베타버전
1. Nightly : 개발버전

### PPA 추가

Ubuntu 17.04버전에서 실행한 결과입니다.

패키지 관리자의 저장소 목록에 파이어폭스 Aurora 채널을 추가합니다

```sh
sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
sudo apt update
```

### Install or Upgrade

우분투 기본 웹브라우저가 Firefox이기 때문에 `apt-get update`를 실행하면 Firefox를 업그레이드 할 수 있다고 나옵니다.(aurora가 더 높은 버전이기 때문에...)

`sudo apt upgrade` 명령어를 통해 설치할 수 있습니다.

파이어 폭스가 없다면 설치합니다.

`sudo apt install firefox`
