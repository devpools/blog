---
title: '[깃헙트렌드]Lottie'
tags:
  - airbnb
  - front-end
  - lottie
id: 651
categories:
  - GitHub-trend
date: 2017-02-09 17:10:47
---

며칠간 깃헙과 프론트엔드 커뮤니티 들에서 Lottie 프로젝트가 들썩들썩 했죠.  오늘은 이  Lottie 에 대해서 알아보겠습니다.

[contentcards url="http://airbnb.design/introducing-lottie/" target="_blank"]

버전은 플랫폼 별( iOS, android, react-native)로 크게 3가지가 있습니다. 모두 최근에 스타가 압도적으로 높아졌습니다. 무엇보다 Airbnb에 대한 기술적인 기대감들이 많이 반영되어 있는 듯 합니다.

[caption id="attachment_803" align="aligncenter" width="648"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170306_065204-1024x886.jpg) 플랫폼 3대장?[/caption]

*   iOS : [https://github.com/airbnb/lottie-ios](https://github.com/airbnb/lottie-ios)
*   android : [https://github.com/airbnb/lottie-android](https://github.com/airbnb/lottie-android)
*   react-native: [https://github.com/airbnb/lottie-react-native](https://github.com/airbnb/lottie-react-native)

## 1\. bodymovin

먼저 이 프로젝트를 이해하기 전에 이해해야 되는 프로젝트가 있습니다. 이름은 bodymovin인데 이 프로젝트는 이른바 디자이너와 개발자의 간격을 줄여주는 프로젝트입니다.

[contentcards url="https://github.com/bodymovin/bodymovin" target="_blank"]

조금 더 설명을 하자면 비주얼 이펙트를 만드는 디자인 프로그램 중에 After Effect가 있습니다. 디자이너와 사이트나 앱 작업을 하다보면 움직이는 gif라던지 mov 파일로 받게 될 때가 있는데 이 프로젝트는 After Effect의 플러그인으로 설치를 할 수 있습니다.

**무엇을 하는 녀석인가요?**

After Effect로 비주얼 이펙트 작업을 하면 저장되는 파일 확장자는 .aep  이지만 gif나 mov로 저장을 할 수 있습니다. 그런데 bodymovin 은 그 움직임을 json 파일로 내려줍니다.

**JSON 파일로 내리면 어떻게 활용할 수 있나요?**

이 bodymovin 에는 플레이어가 존재하는데 웹에서 돌려볼 수 있도록 bodymovin.js 를 제공합니다.

예를 들어 data.json 이라는 형태로 변환을 해서 아래와 같이 코드를 짜면 실행시켜 볼 수가 있습니다.

<pre>var animData = {
 wrapper: document.getElementById('bodymovin'),
 animType: 'html',
 loop: true,
 prerender: true,
 autoplay: true,
 path: 'data.json'

};
 var anim = bodymovin.loadAnimation(animData);</pre>

&nbsp;

[caption id="attachment_663" align="aligncenter" width="188"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/prototype01.gif) 하지만 이것은 하울의 움직이는 gif[/caption]

## 2\. Lottie

Lottie는 이런 컨셉을 가지고 와서 모바일 App에서도 돌아갈 수 있도록 플레이어를 만들었습니다.

[caption id="" align="aligncenter" width="443"]![](http://cfile3.uf.tistory.com/image/271D6A4753330BF92040F8) 미안하다. 이 로티가 아니다.<del>4대악 근절 로티.</del>         출처 : 송파구청 http://smartsmpa.tistory.com/1263[/caption]

안드로이드 기준으로 실행을 한번 해 볼까요?

$git clone https://github.com/airbnb/lottie-android.git

git으로 내려받아서 실행을 시켜보면 샘플 프로젝트가 있다는 것을 확인해 볼 수 있습니다.

[caption id="attachment_805" align="aligncenter" width="257"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170306_065253.jpg) Sample Project[/caption]

[caption id="attachment_806" align="aligncenter" width="284"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170306_065327.jpg) 실행을 시켜봅시다.[/caption]

실행을 시켜보면 아주 경쾌한 화면이 실행됩니다.

[caption id="attachment_667" align="aligncenter" width="176"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/lottie-176x300.gif) spring intro! gif로는 생략이 많이 되네요.[/caption]

Animation  Viewer로 미리 샘플로 등록된 JSON 파일들로 테스트를 해 볼 수 가 있습니다.

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/lotties-pin-176x300.gif)

그리고 파일을 참조할 수도 있으니 외부 URL 같은 부분도 참조해서 서비스할 수 있습니다.

## 3\. 그래서?

이게 어떤 역할을 할 것인지 개발자로써는 사실 확실하지 않아서 "개발바보들 팀"으로 영입이 확실시 되는 UX 디자이너에게 보여주면서 물어봤더니 굉장히 재밌어 하면서 다음과 같은 결론을 내었습니다.

1.  디자이너와 개발자 사이의 협업의 간격이 한층 좁아질 것이다.(움직이는 gif나 mov포맷보다는 훨씬 유연해 질것으로 기대)
2.  하지만 이벤트에 관련된 부분을 처리할 수 있도록 발전하지 않으면 어려울 거 같다.(현재 이벤트는 로딩 될때의 시계열 이벤트만 보입니다. 터치와 영역에 대한 인터페이스가 나오면 좋겠다)

그리고 실제로 우리가 만들어서 JSON을 내보니 아직 레이어가 여러개 일때 처리하는 방법을 잘 모르겠다는 것등의 어려움이 보였습니다. 하지만 간단하게 움직이는 gif의 로고를 처리하는 용도로는 매우 잘 사용할 수 있어 보입니다.

이걸로 프로젝트를 한번 진행하게 되면서 좋은 영감이 떠오르게 되면 다시한번 리뷰해 보도록 하겠습니다.

&nbsp;