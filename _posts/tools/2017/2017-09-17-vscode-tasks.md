---
layout: post
title: VS Code Task이용해서 HTML 파일 실행
categories:
  - Tools
tags:
  - VSCode
  - view HTML
last_modified_at: 2017-09-17T08:12+09:00
comments: true
---

레이아웃을 작성하거나 테스트를 위해서 HTML 파일만 단독으로 실행할 때가 있습니다. VS Code 확장기능으로 웹 브라우저를 실행할 수도 있지만, 약간의 꼼수로 내장되어 있는 Task를 사용해서 HTML 파일을 실행 해보겠습니다.

### 방법

우선 `Ctrl + Shift + P`를 누른 뒤 기본빌드 작업구성을 선택하고 빌드유형을 기타(아무거나 선택해도 상관없음)로 선택하고 다음 내용을 붙여 넣습니다.

```json
{
  "version": "1.0.0",
  "command": "Chrome",
  "isShellCommand": true,
  "windows": {
    "command":
      "C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe"
  },
  "linux": {
    "command": "google-chrome-stable"
  },
  "args": ["${file}"],
  "showOutput": "never"
}
```

Chrome 경로는 적절히 수정해서 사용할 수 있습니다.

사용은 빌드 단축키인 `Ctrl + Shift + B`를 누르면 됩니다.

브라우저 실행을 Task로 인식하다 보니 브라우저가 꺼질때 까지 VS code에서는 작업중으로 인식하는 단점이 있습니다.

### 확장기능 사용

[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
확장기능을 사용하면 hot-reload dev-server를 사용해서 html 파일을 실행할 수 있습니다.
