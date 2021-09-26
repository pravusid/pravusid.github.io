---
layout: post
title: 리눅스 윈도우즈 멀티부팅 환경에서 윈도우즈가 grub에서 사라진 경우
categories:
  - Linux
tags:
  - linux
  - majaro-linux
  - multi-boot
  - grub
  - os-prober
last_modified_at: 2021-03-13T20:10+09:00
comments: true
---

윈도우즈와 리눅스 멀티부팅은 이전 포스트에서 소개한 적 있다.
<https://pravusid.kr/linux/2020/11/08/install-linux-windows.html>

어느날 리눅스를 업데이트 하고나니 [grub](https://en.wikipedia.org/wiki/GNU_GRUB)에서 윈도우즈가 사라졌다.

> 배포판에 따라 기본값으로 윈도우즈 검색이 안되거나, 업데이트로 인하여 설정 값이 변경될 수 있다

## `update-grub`

문제가 생기면 윈도우즈 진입은 안되고 바로 리눅스로 부팅한다.

터미널에서 다음 명령어를 입력해보자.

```sh
sudo update-grub
```

터미널에 출력되는 로그에서 다음 내용이 있는지 확인해보자

> Found Windows Boot Manager on /dev/sdb~

바로 문제가 해결될 수도 있고, 다음과 같은 경고가 출력되면서 윈도우즈가 인식되지 않을 수도 있다.

> Warning: os-prober will not be executed to detect other bootable partitions.

## grub 설정 변경

(선택사항): 진행중 아래 사항을 실행하였어도 해결되지 않는다면, 디스크 관리에 들어가서 윈도우즈 부트 파티션을 마운트 한 뒤 다음 내용을 진행한다.

os-prober 경고를 보았다면 grub 설정을 변경해보아야 한다.

간혹 os-prober가 설치되어 있지 않을 수도 있다

```sh
# arch
sudo pacman -S os-prober
# debian
sudo apt install os-prober
```

### 방법1: 터미널에서 grub 설정 변경

(1) 설정파일을 편집한다

```sh
sudo vim /usr/bin/grub-mkconfig
```

(2-A) 다음 설정을 `true` -> `false`로 변경한다

```conf
# Disable os-prober by default due to security reasons.
GRUB_DISABLE_OS_PROBER="true"
```

(2-B) 설정이 다음 처럼 되어 있다면 주석을 해제한다

```conf
# Disable os-prober by default due to security reasons.
# GRUB_DISABLE_OS_PROBER="false"
```

(3) grub 업데이트

```sh
sudo update-grub
# if not found
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 방법2: grub-customizer 사용

(1) grub-customizer를 설치한다

```sh
# arch
sudo pacman -S grub-customizer
# debian
sudo apt install grub-customizer
```

(2) grub 설정 변경

- grub-customizer를 실행하고
- 일반설정 클릭
- 일반설정 하단의 고급설정 클릭

`GRUB_DISABLE_OS_PROBER` 옵션의 값이 false가 되도록 설정한다

(3) grub 업데이트

```sh
sudo update-grub
```

## reference

<https://askubuntu.com/questions/197868/grub-does-not-detect-windows>
