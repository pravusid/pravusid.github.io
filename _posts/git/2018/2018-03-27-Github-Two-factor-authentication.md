---
layout: post
title: Github Two-factor authentication 사용으로 인한 Authentication failed
categories:
  - Git
tags:
  - Github
  - Two-factor authentication
  - Authentication failed
last_modified_at: 2018-03-07T00:15+09:00
comments: true
---

 보안을 위해서 Github 계정 이중잠금 옵션을 선택했다. 그랬더니 push할때 인증오류가 난다.
분명 계정, 비밀번호는 그대로이고... 변한건 이중잠금밖에 없다. 잠깐 구글링을 했더니 당황스런 결론에 도달했다.

## ssh 사용

https 프로토콜을 사용하는게 문제인가 해서 github에 공개키를 등록하고 ssh로 push하니 작동한다.

## token으로 로그인

https를 사용하려면 token을 사용하면 된다.
Github Settings > Developer Setting > Personal access tokens 메뉴서 repo 권한을 가진 token을 발급받는다.

그리고나서 username은 원래대로, password에 token을 넣으니 해결되었다...

## 해결책은 없는가

매번 token 값을 넣지 않고 해결할 방법이 있으리라 믿지만, 일단 해결책은 다음에 찾고... ssh를 사용해야겠다
