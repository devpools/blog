---
title: 서버리스 아키텍처 소셜미디어 개발기 0편
tags:
  - embed.ly
  - firebase
  - github-page
  - serverless
id: 408
categories:
  - Dev-Story
date: 2016-11-25 12:00:46
---

1.  Firebase에 데이타를 저장하고
2.  소셜 미디어용 카드는 Embed.ly 를 이용해서 만들어
3.  github-page에 자동으로 배포하는

100% 서버리스 아키텍쳐의 소셜 미디어를 만들어 보도록 하겠습니다.

프레임워크는 react를 써 볼까 하고 있습니다.

해당 내용은 진행하면서

https://github.com/ehrudxo/standup

깃헙 프로젝트에 1,2,3,4,5,6,7 편을 branch를 만들면서 진행하도록 하겠습니다. 각각의 소스는 각 branch에서 확인하세요.

최대한 자세한 내용을 적도록 할테니 따라만 오시면 소셜미디어 앱을 만들 수 있도록 하겠습니다.

![1-wtxc57dp0jrgzdpe_iilyg](http://devpools.kr/wordpress/wp-content/uploads/2016/11/1-wTXc57Dp0jRgzDPE_iiLYg-333x1024.png)

### **앱 이름 : 스탠드업!**

<figure id="b700" class="graf graf--figure graf-after--h3">

<div class="aspectRatioPlaceholder is-locked">![](https://cdn-images-1.medium.com/max/1600/1*jVcZpT0H9NU0bd0LKL8w4A.png)</div>

<div class="aspectRatioPlaceholder is-locked">

#### 1\. 개요

제가 일하고 있는 팀은 애자일 개발방법론을 사용하고 있습니다. 지금은 너무나도 많은 회사, 그 안에서도 많은 팀에서 애자일 개발 방법론을 가지고 개발을 하고 있죠?

여러가지 애자일 개발 방법론 중에서도 단연코 가장 많이 사용되는 방법론은 스크럼 혹은 스탠드업입니다. 스크럼이라고 하면 굉장히 넓은 의미의 이터레이션까지 포함하시는 경우가 있으니 스탠드 업에만 국한해서 얘기를 하겠습니다.

> The stand-up has particular value in Agile software development processes, such as Scrum or Kanban, but can be utilized in context of any software-development methodology.

출처 : [stand-up meeting](https://en.wikipedia.org/wiki/Stand-up_meeting)(미디어 위키)

꼭 애자일이 아니더라도 소프트웨어 개발을 위해서는 종종 사용되는 방법이라고 하고 타임박싱은 5분에서 15분 정도를 합니다.

나머지는 조금씩 다를 수 있겠지만, 10여명이 넘는 팀 전체 스탠드업에는 각 나눠진 작은 팀의 주제보다는 팀 전체를 위한 이슈를 이야기 하는데요. 머리를 말랑말랑하기 위해 간단한 IT 이야기를 할 수 있는 앱을 만들어 볼까 합니다.

</div>

</figure>

![1-ogda8oko4yxo-q0jfruc0g](http://devpools.kr/wordpress/wp-content/uploads/2016/11/1-OgDA8okO4Yxo-Q0jFRuc0g-1024x682.png)

기왕이면 아침에 스마트폰으로 볼 수 있도록 모바일을 먼저 고려하는 것도 당연히 들어가야 합니다.

#### 2\. 설계

*   **페르소나**
이름 : 김개발 ![1-jyaayq7fntjcm9fuagazqw](http://devpools.kr/wordpress/wp-content/uploads/2016/11/1-jyaaYQ7fntJcm9fuagaZqw-1024x711.png)

애자일 개발자 김개발 씨는 IT 트렌드에 무척이나 민감하다. 아침 스탠드 업 시간에 일간 보고만 하는 것이 매우 못마땅한데 기술적인 이야기를 같이 하면 좋겠다.

*   **User Story**

> 1\. 김개발은 아침 스탠드업 시간에 같이 이야기를 나눌 수 있는 주제를 위해 스탠드업이라는 웹 앱을 찾아간다. 이렇게 함으로써 사람들과 IT에 대한 주제로 커뮤니케이션을 할 수 있다.

> 2\. 김개발이 사이트를 방문해서 자신이 어제 유심하게 읽은 글을 올릴 수 있다. 이렇게 하면 다른 사람들이 볼 수 있다.
> * 에디터 창은 하나만 있고 거기서 글을 작성하고 업로드 하면 글이 외부 클라우드 공간에 저장이 된다.

> 3\. 김개발이 작성한 글이 목록으로 보여진다. 이렇게 함으로써 다른 사람들이 목록을 확인할 수 있다.
> * 목록글은 해당IT 주제의 대표 이미지와 제목, 간단 요약등이 들어 있는 카드의 리스트 형태로 나열이 되어야 한다.

유저스토리는 개발 과정 중에 변경이 일어날 것인데, 그런 과정도 중간 중간 보 언급하면서 넘어가겠습니다.

#### 3\. 개발전 준비

*   깃헙에 프로젝트 만들기

<figure id="7b62" class="graf graf--figure graf-after--li">

<div class="aspectRatioPlaceholder is-locked">![1-nkzv8_g235yyfvmaqvi1da](http://devpools.kr/wordpress/wp-content/uploads/2016/11/1-nKzv8_G235YyfvmAQvI1DA-1024x902.png)</div>

<div class="aspectRatioPlaceholder is-locked">

위와 같이 프로젝트를 만들어 줍니다. 물론 fork를 따는 방법도 있겠지만(그래주시면 일단은 감사! star도 한번 눌러주시면 더 감사!) 따라가는 입장에서는 만들어 보는 방법도 좋은 시도 입니다.

만들고 나면 아래와 같이 설정창이 뜨게 됩니다.

![1-hzyx2-d12f2iin-lp5jy6g](http://devpools.kr/wordpress/wp-content/uploads/2016/11/1-HZyx2-D12f2IiN-lP5jY6g-1024x729.png)

*   README.md파일 만들기

Readme 파일은 Art-of-readme 라는 내용을 참고하시면 더 좋을 거 같습니다.

https://techstory.shma.so/art-of-readme-cd19f86b0456#.ovu6sws90

&nbsp;

</div>

</figure>

&lt;Readme.md파일&gt;

# StandUP!

    애자일 프랙티스 standup 할 때 아이스브레이킹에 사용되는 IT기술에 관련된 주제를 나눌 수 있는 아티클들을 공유하는 소셜미디어 프로젝트
    `</pre>

    ## UserStory

1.  김개발은 아침 스탠드업 시간에 같이 이야기를 나눌 수 있는 주제를 위해 스탠드업이라는 웹 앱을 찾아간다. 이렇게 함으로써 사람들과 IT에 대한 주제로 커뮤니케이션을 할 수 있다.
2.  김개발이 사이트를 방문해서 자신이 어제 유심하게 읽은 글을 올릴 수 있다. 이렇게 하면 다른 사람들이 볼 수 있다.  * 에디터 창은 하나만 있고 거기서 글을 작성하고 업로드 하면 글이 외부 클라우드 공간에 저장이 된다.
3.  김개발이 작성한 글이 목록으로 보여진다. 이렇게 함으로써 다른 사람들이 목록을 확인할 수 있다.  * 목록글은 해당IT 주제의 대표 이미지와 제목, 간단 요약등이 들어 있는 카드의 리스트 형태로 나열이 되어야 한다.

    ## 사용법

    아직 빌드중이지만 아래와 같이 설계됨

    ### 설치

    <pre>`$npm install
    `</pre>

    ### 실행

    <pre>`$npm start
    `</pre>

    이렇게 실행하고 http://localhost:3000 으로 접속하시면 개발 버전을 확인할 수 있습니다.

    ## API

    API는 계획없는 자체 프로젝트 입니다. 추후 발생할 여지는 있음.

    ## production

    제품을 빌드하려면

    <pre>`npm run build
    `</pre>

    제품을 디플로이 하려면

    <pre>`npm run deploy

를 통해 작업할 수 있습니다.

### 라이센스

MIT

&lt;Readme.md파일 끝&gt;

<figure id="7b62" class="graf graf--figure graf-after--li">

<div class="aspectRatioPlaceholder is-locked">

의 형태와 같이 README.md 파일을 만들고 깃에 푸쉬하고 준비과정은 마무리 하도록 하겠습니다.

</div>

</figure>