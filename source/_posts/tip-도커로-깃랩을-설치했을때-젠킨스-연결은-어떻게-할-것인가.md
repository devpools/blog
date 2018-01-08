---
title: '[tip] -도커로 깃랩을 설치했을때 젠킨스 연결은 어떻게 할 것인가'
id: 4516
comment: false
categories:
  - 'tip, docker, devops, git'
date: 2017-09-14 13:31:23
tags:
---

깃랩같은 좋은 툴은 언제나 파워풀하지만, 설치가 쉽지않다. 그래서 깃랩을 설정하기가 어려운 부분은 도커가 최근에는 대신하고 있다.
그런데, 도커로 설치를 하다보면 CI, CD 환경을 꾸미는 것은 어떻게 해야할지 감이 오지 않을 때가 많다.

도커로 깃랩을 설치하는 것과 관련된 글은 검색을 하면 수십개가 나오고, 젠킨스와 깃랩을 연동하는 부분도 굉장히 많이 나오지만 도커로 설치된 깃랩과 젠킨스 연동을 위한 중요한 링크가 빠져 있다.

중요한 두가지 설정 포인트가 필요하다.

1.  ssh 포트 변경된 경우 git 연결
~/.ssh/config 파일을 만든다.
ServerName, UserName, Port 은 프로젝트에 맞춰 준다.<figure class="highlight bash"><table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div></pre></td><td class="code"><pre><div class="line">Host &#123;ServerName&#125;</div><div class="line">        User &#123;UserName&#125;</div><div class="line">        Port &#123;Port&#125;</div></pre></td></tr></table></figure>

이후 git 명령어로 프로젝트를 잘 가져오는지 확인하자.
<figure class="highlight bash">

<table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div></pre></td><td class="code"><pre><div class="line"><span class="variable">$git</span> <span class="built_in">clone</span> ssh://git@server:[port]/url </div><div class="line">`</div></pre></td></tr></table>

</figure>

1.  git 연결 주소 설정
참고로, 퍼블릭키와 프라이빗 키는 이미 잘 설정되어 있을 거라는 전제하에 얘기한다.

링크 : [gitlab과 Jenkins연동](http://egloos.zum.com/mcchae/v/11246199)

위 링크에 대해서 URL을 설정하는 부분만 아래와 같이 바꾸면 잘 해결되는 것을 확인할 수 있다.

![링크 주소 설정](http://keen.devpools.kr/images/sourcecodemgmt.png)

이렇게 되고 나면 깃에 자유자재로 업데이트하고 긁어올 수 있음을 알 수 있다. 나머지 CI, CD 옵션은 원하는데로 구성하면 된다.