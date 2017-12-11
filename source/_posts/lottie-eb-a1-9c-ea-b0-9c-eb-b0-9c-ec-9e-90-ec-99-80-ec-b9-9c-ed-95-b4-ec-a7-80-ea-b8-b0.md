---
title: Lottie로 개발자와 친해지기
tags:
  - lottie
id: 852
categories:
  - UX/UI
date: 2017-03-08 09:50:55
---

![](http://airbnb.design/lottie/static/images/lottie.png)

'마이크로 인터랙션'이라는 단어가 2015년부터 UX 디자인에서 조명받기 시작하면서
(마이크로 인터랙션의 효과에 대한 글은 [이 곳을 참조](https://visualhierarchy.co/blog/micro-interactions-what-they-are-how-they-work-and-best-practices/))
우리는 Behance나 Dribbble에서 아름다운 UI 모션 GIF들을 어렵지 않게 발견할 수 있다.

&nbsp;

[caption id="" align="alignnone" width="800"]![항공표와 좌석을 예매하는 시스템 이미지](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/1d4s/image/m9-mFChtXzv8T0FExV1Ou2PD8ac.gif) 출처 : https://dribbble.com/eleken[/caption]

 그러나 실제 프로젝트에 이런 것들을 적용하려고 하면 다음과 같은 장벽에 부딪치게 된다.

### ** 1\. 디자이너가 원하는 모션을 만들 줄 모른다. **

디자이너가 원하는 모션을 만들기 위해서는 AfterEffect와 같은 전문 모션 툴을 사용할 줄 알아야 하는데,
제대로 쓰기 위해 상당히 많은 배움이 요구된다. 그렇기 때문에 본인이 원하는 모션과 가장 유사한 레퍼런스를 찾아 개발자에게 "비슷하게 해주세요~" 해야 하는데.. 개발자들은 멘붕이 올 수밖에 없다.

&nbsp;

![김정은 도리도리하는 모습](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/1d4s/image/9lA13_r-XDmNT7Mq4cvw4hjcUPw.gif)

위의 이미지를 개발자들에게 들이 댔을때 표정

### ** 2\. 개발자는 디자이너가 원하는 모션이 무엇인지 정확히 모른다. **

마침 디자이너가 AE를 다룰 줄 알아서 개발자에게 GIF 파일을 만들어줬다고 하자. 개발자들은 이미지만 보고 '알아서' 그 모션을 구현해야 한다. 위의 이미지를 예로 들면 리스트가 펼쳐질 때의 시간, 크기, 투명도를 스스로 체크하거나 디자이너에게 수치를 받아서 모든 액션을 다시 구현해야 하는 '낭비'가 발생하게 되는 것이다.

대안으로 디자이너가 만들어준 GIF 파일을 코드에 직접 삽입하는 방법이 있는데,  [GIF는 태생적으로 한계를 가진 이미지 포맷](https://medium.com/@soeunlee/web%EC%97%90%EC%84%9C-png-gif-jpeg-svg-%EC%A4%91-%EC%96%B4%EB%96%A4-%EA%B2%83%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%A2%8B%EC%9D%84%EA%B9%8C%EC%9A%94-6937300e776e#.s9y9amz6b)이고 용량, 디바이스 해상도 대응 이슈 등 극복해야 할 단점들이 많다.

### ** 3\. 모두가 힘들어지는데 굳이 해야 하나?**

이런 이유들로 시간과 비용을 따져보면 UI에 모션을 넣는 애니메이션 작업은 팀 전체 관점에서 결코 효율적인 작업은 아니다.

특히 팀 속도와 낭비제거를 중점으로 삼는 Agile 프로세스 내 User Story 기반 작업에서 우선순위가 낮을 수밖에 없다. 결국 디자이너가 원하는 마이크로 인터랙션들을 넣기 위해서는 디자이너가 만든 이미지와 모션을 다시 개발자가 코드화 하는 낭비를 줄일 수 있는 방법이 있어야 한다는 것이다.

### ** 4\. Airbnb의 'Lottie'**

 그런 가려운 부분을 긁어주기 위해 출시된 라이브러리가 [Airbnb의 Lottie](https://github.com/airbnb/lottie-react-native)라는 것이다. 자세한 내용은 [이전 글을 참조](http://devpools.kr/2017/02/09/%EA%B9%83%ED%97%99%ED%8A%B8%EB%A0%8C%EB%93%9C-lottie/)해보자.

요컨대, 디자이너가 AE로 만든 모션을 바로 JSON파일로 Export 하고 그것을 다시 개발 코드에 삽입할 수 있게 된다는 것이다. 설치와 사용 방법은 [Github](https://github.com/bodymovin/bodymovin)에 상세히 써져 있으니 참고하도록 하자

### ** 5. Lottie의 적용**

마침 지금 살짝 도와주고 있는 토이 프로젝트가 있어서 간단한 스플래시 화면을 위한 AE 프로젝트를 만들어 개발자에게 전달해보았다.

[caption id="" align="alignnone" width="288"]![아저씨가 웃고 있는 모바일 앱의 스플래시 화면](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/1d4s/image/-1XTNiwfPpI14qPudZ9sVwVgc8U.gif) 일 안하고 놀고 있다는 것을 들키고 말았다..[/caption]

하지만 실제 코드에 삽입했을 때는 프레임 드랍 현상이 발생하며, AE의 Repeater로 만든 애니메이션은 반영되지 않았다. 관련하여 Github에 [Repeater](https://github.com/bodymovin/bodymovin/issues/209), [Frame Drop ](https://github.com/airbnb/lottie-android/issues/151)에 대한 이슈가 등록되어 있다. 결론적으로 무작정 시도해봤었는데 현재 Bodymovin에서 Json 파일로 추출할 수 있는 AE 플러그인에는 한계가 있으며, 버전업을 통해 그 범위를 차차 늘려가고 있는 중인 것으로 보인다. 현재 이슈로 등록되어있는 내용을 정리하면 다음과 같다.

1\. [Edge 브라우저에서 Luma와 Alpha 마스크가 적용되지 않는 현상](https://github.com/bodymovin/bodymovin/issues/295)

2\. [Trim path로 그렸을 때 정확한 위치를 가져오지 못하는 현상](https://github.com/bodymovin/bodymovin/issues/287)

3\. [Blur 적용 문제](https://github.com/bodymovin/bodymovin/issues/283)

4\. [3D 카메라 적용 버그](https://github.com/bodymovin/bodymovin/issues/260)

5\. [Glow effect 미지원](https://github.com/bodymovin/bodymovin/issues/215)

6\. [Repeater Effect 미지원](https://github.com/airbnb/lottie-ios) (곧 지원 예정)

다른 이슈들도 상당히 많이 등록되어 있고 활발한 토론이 이뤄지고 있지만 코드 차원의 문제가 아니라 AE 자체 이펙트를 지원하지 않는 것은 거의 위와 같은 마스크와 스트로크(<span style="text-decoration: line-through;">사실 이게 핵심이긴 한데..</span>) 유형이었다.

그럼에도 불구하고, 애니메이션을 만들고 개발자에게 전달하여 구현까지 상당히 빠른 시간 내에 적용해볼 수 있었다. (30분도 안 걸린 것 같다...!!) 이전에 영상파일로 전달하여 구현한 파일을 몇 번이나 확인하며 수정과정을 거쳤던 예전과 비교해 볼 때, json 파일만 전달하면 되기 때문에 나는 더 이상 애니메이션에 대해 개발자랑 토론할 필요도 없었다. (내가 만든 애니메이션을 코드로 이해해야 하는 것도 부담이었다.)

[caption id="" align="alignnone" width="250"]![낄낄 웃고있는 외국배우](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/1d4s/image/1jikcKe6UrMh-bOm41bErpIqMV8.gif) 개발자에게 결과물을 받아보는 나의 모습[/caption]

### **결론 **

위에 언급한 대로 디자이너는 사용자에게 더 많은 것을 보여주고 싶어 한다. 하지만 실제 프로젝트에서는 시간과 인력으로 대표되는 한정된 자원을 가지고 있기 때문에 내가 원하는 애니메이션까지 구현하는 것은 거의 불가능에 가까웠었다. 그러나 이렇게 효율적으로 애니메이션을 구현할 수 있는 라이브러리가 등장함에 따라 이제 앞으로는 Dribbble이나 Behance에서 볼 수 있는 화려한 애니메이션들이 실제 제품에 적용되어 출시될 수 있는 길이 열린 것 같다.

흔히 시간과 인력으로 대표되는 한정된 자원이라는 관점에서 이렇게 효율적으로 작업할 수 있는 방법이 지속적으로 개선되고 있다는 것은 디자이너로서 또 한 번 반길 일이 아닐 수 없으며, 고이 책장에 모셔<span style="text-decoration: line-through;">(쳐 박아)</span> 두었던, 모션그래픽 관련 책들을 다시 들춰봐야 할 때가 아닌가 싶다.