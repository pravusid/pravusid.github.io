---
layout: post
title: 구글 한글 웹폰트
categories:
  - Lifelog
tags:
  - 구글 폰트
  - Noto sans KR
  - 한글 웹폰트
last_modified_at: 2018-04-24T22:10+09:00
comments: true
---

구글에서 Early Access로 공개했던 한글 웹폰트를 본격지원하기 시작했다.

기존에 <http://fonts.googleapis.com/earlyaccess/> Early Acess로 공개되었으나, 최근 정식제공 소식이 들려왔다.

<https://googlefonts.github.io/korean/> 에서 지원하는 폰트와 제작과정에 대한 설명 등을 볼 수 있다.

## 적용하기

구글 설명에 따르면 CJK(한중일) 폰트는 용량이 크기 때문에(한자 + 완성형 한글) 분할 다운로드를 적용하였다고 한다.

분할 다운로드에 머신러닝을 적용하여, 해당 css를 요청하는 곳이 늘어날 수록 최적화 된다고 한다.

폰트 두께도 선택해서 가져올 수 있다. 요청한 두께가 없으면 제외하고 가져오는 듯 하다.

우선 구글 api로 부터 폰트를 가져와야 한다. 즐겨쓰는 폰트 세 가지를 요청하는 주소는 다음과 같다.

```css
/* 구글 Noto Sans KR */
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+KR:400,700');

/* 네이버 Nanum Gothic */
@import url('https://fonts.googleapis.com/css?family=Nanum+Gothic:400,700');

/* 배민 Do Hyeon */
@import url('https://fonts.googleapis.com/css?family=Do+Hyeon:400');
```

<https://googlefonts.github.io/korean/> 에서 지원하는 폰트이름을 호출하면, 다른 폰트도 사용할 수 있다.

폰트 적용은 CSS를 불러온 뒤 CSS에서 Font Family 속성을 지정하면 된다.

## 저작권

구글 폰트에서 제공하는 폰트는 공개폰트이다 (대부분 Open Font License 또는 Apache 2.0 그 외 기타...)

OFL 라이센스 경우에는 공개된 폰트를 단독으로 재판매 하는것만 제외하고, 상업적 사용, 임베드, CI에 사용 등 대부분을 허용한다.

공개 한글 글꼴들의 개별 저작권은 한국어 위키피디아에서 찾아볼 수 있다.

<https://ko.wikipedia.org/wiki/%ED%8B%80:%EA%B3%B5%EA%B0%9C_%ED%95%9C%EA%B8%80_%EA%B8%80%EA%BC%B4>
