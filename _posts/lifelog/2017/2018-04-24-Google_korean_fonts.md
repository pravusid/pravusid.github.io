---
layout: post
title: 구글 한국어 웹폰트
categories:
  - Lifelog
tags:
  - 구글 폰트
  - Noto sans KR
  - 한국어 웹폰트
last_modified_at: 2018-04-24T22:10+09:00
comments: true
---

구글에서 Early Access로 공개했던 한국어 웹폰트를 본격지원하기 시작했다.

기존에 <http://fonts.googleapis.com/earlyaccess/> 주소에서 font관련 css를 가져왔으나 정식제공 소식이 들려왔다.

<https://googlefonts.github.io/korean/> 에서 지원하는 폰트와 제작과정에 대한 설명 등을 볼 수 있다.

## 적용하기

우선 구글 api로 부터 폰트를 가져와야 한다. 즐겨쓰는 폰트 세 가지를 요청하는 주소는 다음과 같다.

구글 설명에 따르면 CJK(한중일) 폰트는 용량이 커서 분할 다운로드를 적용하였다고 한다. 폰트 두께도 골라서 가져올 수 있다.

```css
/* 구글 Noto Sans KR */
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+KR:400,700');

/* 네이버 Nanum Gothic */
@import url('https://fonts.googleapis.com/css?family=Nanum+Gothic:400,700');

/* 배민 Do Hyeon */
@import url('https://fonts.googleapis.com/css?family=Do+Hyeon:400,700');
```

<https://googlefonts.github.io/korean/> 에서 지원하는 폰트명을 호출하면, 다른 폰트도 사용할 수 있다.

폰트 적용은 CSS를 불러온 뒤 CSS에서 Font Family 속성을 지정하면 된다.

## 저작권

구글 폰트에서 제공하는 폰트는 대부분 공개폰트이다 (Open Font License, Apache 2.0)

상업적사용까지 큰 제약이 없는 경우가 대부분이다.

공개 한글 글꼴들의 개별 저작권은 한국어 위키피디아에서 찾아볼 수 있다

<https://ko.wikipedia.org/wiki/%ED%8B%80:%EA%B3%B5%EA%B0%9C_%ED%95%9C%EA%B8%80_%EA%B8%80%EA%BC%B4>
