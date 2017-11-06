---
layout: post
title: Nord Syntax Theme
categories:
  - Tools
tags:
  - nord
  - syntax theme
last_modified_at: 2017-08-28T22:27+09:00
comments: true
---

## Syntax Theme

IDE나 텍스트 에디터를 사용하면 기본 구문 하이라이팅도 충분히 좋은 경우가 많습니다.
하지만 개인적인 취향에 맞춰 색 조합을 바꾸는 일도 필요하고, 여러 환경에서 동일한 경험을 얻는 것도 중요합니다.

### 기존에 사용하던 테마들 입니다

- 기본테마 : IDE나 Editor를 처음 실행하면 흰색 배경에 초록, 파랑, 빨강색이 주가 되는 기본테마를 주로 접할 수 있습니다.

- Obsidian : 흑요석이라는 이름답게 짙은 회색의 배경을 바탕으로 초록, 오렌지, 파랑, 흰색의 글자색 조합이 주를 이룹니다.
  대부분의 IDE, Editor에서 기본으로 지원하거나, 만들어져 있는 테마를 활용해서 사용할 수 있습니다.

- Darcula : Jetbrains IDE에서 기본으로 지원하는 Dark theme로 유명합니다.
  Jetbrains 제품이 아니더라도 유저들이 만들어 놓은 테마를 찾아서 이용할 수 있습니다.
  Obsidian 보다 밝은 배경에 붉은 빛의 Highlighting이 많습니다.

기존의 테마도 마음에 들었지만 다른 제작자들이 미세하게 다른 컬러 팔레트를 가지고 같은 이름으로 테마를 만드는 경우가 많았습니다.
또한, 유명 테마의 경우에도 일부 IDE나 에디터에 없는 경우도 있기 때문에 대부분의 상황에 대응할 수 있는 테마를 찾아다녔습니다.

위의 조건을 모두 만족하는 테마는 거의 없었고 세 가지의 후보로 좁힐 수 있었습니다.

### 세 개의 후보

- [Material Theme](https://github.com/equinusocio/material-theme) :
  Gooogle Android 5.0 이후의 디자인 테마 Material에서 영향을 받아 만들어진 Syntax Theme입니다.
  Sublime Text Theme로 인기를 얻고 나서, 대부분의 Text Editor나 IDE에서도 사용할 수 있는 것으로 보입니다.

  ![Material Theme](https://camo.githubusercontent.com/972bd5d93779fdaf95e02cf0326b429be93adcba/687474703a2f2f692e696d6775722e636f6d2f395079784a4d4e2e676966)

- [Dracula](https://draculatheme.com/) :
  Darcula와 이름이 거의 유사하지만 서로 다릅니다. 드라큘라는 여러 테마들 중 가장 많은 IDE, 에디터, 터미널을 일관성있게 지원하고 있습니다.
  심지어 메신저 테마도 지원하고 있습니다.

  ![Dracula Theme](https://draculatheme.com/assets/img/screenshots/vscode.png)

- [Nord](https://git.io/nord) :
  조건에 맞춰 찾은 세 테마중 가장 늦은시기에 제작되었습니다.
  대부분의 Syntax Theme가 다양한 색을 사용해서 알록달록한 느낌을 주는데 비해 단조롭고 낮은채도의 색상을 사용한 것이 특징입니다.

  ![Nord Theme](https://raw.githubusercontent.com/arcticicestudio/nord-visual-studio-code/develop/assets/scrot-preview.png)

## Nord Theme

컬러 팔레트는 Material과 Nord가 마음에 들었고, 에디터 지원 정도는 Dracula와 Nord가 좋았습니다.
평점 총량에 따라서 Nord를 사용하기로 마음 먹었습니다.

<https://github.com/arcticicestudio/nord>를 방문하시면 지원하는 IDE와 에디터, 터미널의 목록을 볼 수 있는데 굉장히 많습니다.
목록에 있지 않은 [Notepad++](https://github.com/arcticicestudio/nord-notepadplusplus) 테마도 만들어져 있습니다.
사용자 수가 그리 적지도 않고 Atom, VScode 에디터에서 사용자가 꾸준히 늘고 있어서 프로젝트가 버려질 위험도 별로 없어 보입니다.

최근 많은 관심을 받고 있는 Modern Editor 삼총사 Sublime Text, Atom, VSCode의 경우에는 기본 테마도 충분히 훌륭하고,
사용성을 고려해보면 - 어두운 테마 뿐만 아니라 밝은 테마까지 본다면 -
[Material Theme](https://github.com/equinusocio/material-theme)가 가장 무난한 선택으로 보입니다.