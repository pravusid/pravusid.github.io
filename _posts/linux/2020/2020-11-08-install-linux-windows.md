---
layout: post
title: Windows & Linux 멀티부팅 설정
categories:
  - Linux
tags:
  - linux
  - windows
  - multi-boot
last_modified_at: 2020-11-08T22:22+09:00
comments: true
---

리눅스를 설치하면서 필요한 사항을 간단히 정리해보았다.

멀티부팅을 위한 내용들이지만 단일부팅을 하더라도 공통으로 적용할 수 있다.

## Bootable USB

> 직접 부트가능 USB를 제작할 수도 있지만 [Ventoy](https://www.ventoy.net/en/index.html)를 사용하는 것이 더 좋다

<https://wiki.archlinux.org/title/USB_flash_installation_medium>

```sh
sudo fdisk -l
sudo dd bs=4M if=path/to/linux-version-x86_64.iso of=/dev/sd(drive letter) conv=fsync oflag=direct status=progress
```

## 바이오스 설정

바이오스에서 Secure Boot disabled, Fast Boot disabled 설정

## 파티션

기본적으로 설치순서는 `윈도우즈 > 리눅스`를 따르는 것이 좋다.

윈도우즈 부트 매니저에서는 리눅스 부트영역을 인식하지 않지만,
grub에서는 윈도우즈 부트영역을 인식하여 멀티부팅 처리를 해 주기 때문이다.

### 단일 디스크

- 윈도우즈 설치
- 윈도우즈에서 볼륨 축소하여 리눅스에서 사용할 용량 확보
- 리눅스 Bootable USB로 설치

> ESP(EFI System Partition)는 디스크에 단 하나만 있어야 되고, 윈도우즈의 ESP가 있다면 그것을 `/boot/efi`로 마운트 한다

윈도우즈를 UEFI 환경에서 설치했다면

- 100mb 크기의 efi 파티션에 윈도우즈 부트로더가 위치한다
- **부트로더를 설치할 장치**를 부트로더가 위치한 efi 파티션으로 선택

윈도우즈를 EFI(Legacy) 환경에서 설치했다면

- 500mb 크기의 ntfs 파티션에 윈도우즈 부트로더가 위치한다
- **부트로더를 설치할 장치**를 운영체제가 위치한 디스크(`/dev/sda`)로 선택

### 개별 디스크

1, 2번 디스크가 있다고 가정했을 때 기본 설정대로 설치를 진행해도 된다

- 2번 디스크에 윈도우즈 설치 (부트 파티션, 운영체제 파티션 및 복구 파티션이 생성됨)
- 1번 디스크에 리눅스 설치 (부트 파티션의 grub에서 윈도우즈 부트 매니저를 읽어와서 처리함)

## grub 설정

두 운영체제를 모두 설치했으나 Grub2 화면에서 윈도우즈가 보이지 않을 수 있다

이 경우 Grub 설정을 변경해야 한다: <https://pravusid.kr/linux/2020/11/08/install-linux-windows.html>

## 스왑 용량

- <https://itsfoss.com/swap-size/>
- <https://help.ubuntu.com/community/SwapFaq>

리눅스 용도가 데스크탑 환경 활용이라면 스왑이 전혀 필요하지 않을 수도 있다.
그러나, 최대절전(hibernation)을 사용한다면 메모리 용량을 초과하여 스왑영역을 잡아야 한다.

일반 용도와 일반 램용량(16gb~32gb)이라면 우분투 문서에서 추천하는 대로 메모리 용량의 `1/4` 정도로 설정하는 것이 좋을듯 하다.

## 시간 동기화

두 운영체제를 번갈아가며 실행하다보면 가끔 시계가 잘못 표시되는 경우가 발생한다.
이는 메인보드의 RTC(Real Time Clock)를 다루는 방식이 달라서 그렇다.

Windows에서는 RTC를 Local Time으로 처리하여 동기화 하고, 리눅스에서는 UTC로 처리하여 동기화 하므로,
한 운영체제에서 시간 동기화를 실행하는 순간 다른 운영체제에서 잘못된 시간이 출력된다.

가장 간단한 방법은 리눅스에서 RTC를 로컬타임으로 처리하도록 설정하는 것이다.

```sh
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```
