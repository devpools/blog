---
title: 4월 1주차 해커뉴스 소식
id: 262
categories:
  - 미분류
date: 2016-04-04 20:02:02
tags:
---

1.  [NPM and Left-Pad: Have We Forgotten How to Program? (haneycodes.net)](http://www.haneycodes.net/npm-left-pad-have-we-forgotten-how-to-program/)

"left-pad"라는 npm모듈과 React, Babel 그리고 다른 패키지들과의 의존성 때문에 많은  npm 패키지들이 깨지는 현상이 발견되었습니다.

이 문제 때문에?(덕분에?) 진행된 npm에코시스템에 대해 저자가 관찰한 사항에 대한 이야기 입니다.
2\. [I've Just Liberated My Modules (medium.com)](https://medium.com/@azerbike/i-ve-just-liberated-my-modules-9045c06be67c)

[Azer Koçulu](https://medium.com/@azerbike)의 글인데요, 뭔가 글에서 화남이 느껴집니다. kik 이라는 이름의 npm 모듈을 만들고 있었는데 변호사에게 연락이 와서 이미 등록된 게 있으니 사용하지 말라고 압박이 들어왔습니다. 거절의사를 밝히자 [@izs(npm의 CEO)](https://twitter.com/izs)는 저자의 허가 없이 모듈의 오너쉽을 바꾸는 걸 허락했네요. 정말 어처구니 없는 일이네요.

저자는 더이상 npm에서 소스를 공유할 수 없음을 판단하고 다른 모든 모듈도 npm에서 출판하지 않을 것이라고 선언했네요. 앞으로 Azer의 소스를 사용한다면 [Github](https://github.com/azer)을 이용해 주세요:)
3\. [That awkward moment when Apple mocked good hardware and poor people (techinasia.com)](https://www.techinasia.com/awkward-moment-apple-mocked-good-hardware-poor-people)

Apple이 좋은 하드웨어와 가난한 사람들을 조롱할 때 꼴사나운 순간? 정도로 번역할 수 있을까요? 이런 직역으로는 내용 파악이 바로 불가능할 것 같네요 ㅜ.ㅜ

Apple의 사용자들은 upgrade를 할 필요성을 느끼지 않는다. 사용자가 upgrade를 하게 만들기 위해서는 "이유"를 만들어 주어야 한다는 내용이네요.
4\. [Left-pad as a service (left-pad.io)](http://left-pad.io/)

첫번째 해커뉴스와 같은 맥락의 글입니다. 아까 말했듯이 "left-pad"모듈 때문에 패키지가 깨지는 현상이 발생했습니다. nodejs는 쿨하게 "left-pad"모듈을 삭제했습니다. 역시 프로개발자는 빈틈을 놓치지 않죠!!! 한 개발자가 몇일 되지 않아 left-pad.io 라는 걸 만들어냈습니다. (라이센스 부분 읽어보면 엔터프라이즈 라이센스는 유료인 듯 합니다.)
5\. [Docker for Mac and Windows Beta (blog.docker.com)](https://blog.docker.com/2016/03/docker-for-mac-windows-beta/)

[embed]https://youtu.be/9CuClvKMt04[/embed]

도커의 3번째 생일을 맞아 리눅스에서만 사용가능하던 Docker의 윈도우 버전과 맥버전이 출시되었습니다! 아직 베타버전이긴 하지만요 :)
6\. [I switched to Android after 7 years of iOS (joreteg.com)](https://joreteg.com/blog/why-i-switched-to-android)

제목만 봐도 너무 궁금해 지는 이야게네요. iOS를 7년이나 사용하고 안드로이드로 바꾸려는 이유는 무엇일까요?

[Progress Web App](https://developers.google.com/web/progressive-web-apps?hl=en)의 프로토타이핑을 시연하기 위해 오래된 안드로이드 폰을 꺼내다가 자신이 현재 가지고 있는 iPhone6보다 구형의 안드로이드가 웹어플리케이션을 위한 플랫폼으로는 더 뛰어나다고 생각했다고 합니다.

관점에 따른 차이겠지만 Henric Joreteg는 요즘은 많은 앱들이 웹앱으로 변모하고 있고, 웹이야 말로 유일한 개방형 플랫폼이라고 생각해 웹앱에 초점을 두고 이야기를 한 듯 합니다.
7\. [Citus Unforks from PostgreSQL, Goes Open Source (citusdata.com)](https://www.citusdata.com/blog/17-ozgun-erdogan/403-citus-unforks-postgresql-goes-open-source)

![](https://www.citusdata.com/sites/default/files/animated_citus.svg)

Citus 5.0 버전이 나왔다는 기사네요. Citus는 PostgreSQL을 빅데이터 어플리케이션이 요구하는 분산 데이터베이스로 사용할 수 있게하고, 실시간 읽기/쓰기와 대용량 병렬분석 기능을 제공합니다.  PostgreSQL의 최신버전 API를 직접사용하는 세계 최초 분산 데이터베이스이고 오픈소스로 github에서 확인할 수 있습니다.

1.  [Require-from-Twitter (gist.github.com)](https://gist.github.com/rauchg/5b032c2c2166e4e36713)

![](https://i.imgur.com/VY6BCZp.png)

github gist에 올라온 글입니다. github gist가 뭔가 했는데 repository를 만들지 않고 간단한 코드만 올릴때 사용하는 코드 게시판 같은 거였네요. 블로그에 직접 임베드 시킬 수 있습니다. require-from-twitter는 twitter 패키지 매니저입니다. 이게 왜 올라왔나 봤더니 이것도 LeftPad 가 포함되어서 그런 것 같네요.