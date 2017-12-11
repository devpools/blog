---
title: 주간 깃헙 트렌드 2016 년 3월 첫호
id: 146
categories:
  - 미분류
date: 2016-03-14 19:32:22
tags:
---

![](http://i.imgur.com/KxXNI51.png)

&nbsp;

주간 깃헙트렌드 2016년 3월 첫호입니다.

이번주에 리뷰할 순위에 들어있는 프로젝트에는 AlphaGo 가 올라와 있습니다. ( 구글 딥마인드의 그 알파고는 아닙니다만, 같은 논문을 통해 작성되었다고 하니 관심을 가져봐도 될 것 같습니다.

# Machine Learning

이세돌과 알파고의 대전이 많은 사람들에게 이목을 많이 집중시키고 있는 것 같습니다. 단지 우리나라에만 이 사건이 집중적인 것 같지는 않습니다. 6위 안으로 한정 시켜보면 FreeCodeCamp가 약간 like 버튼을 growth hacking 하면서 커지는 걸 감안해 보면 status page 를 포함한 두가지 말고는 모두 Machine Learning에 대한 프로젝트라고 볼 수 있습니다 @.@.

#### <span style="color: #808080;">AlphaGo</span>

![](https://cdn-images-1.medium.com/max/2000/1*15xWjTnuN6nYA0UYO7O9EA.png)

> 왠걸 이럴 줄 알았습니다. 이틀만에 AlphaGo를 오픈소스로 만들겠다는 친구가 나왔네요. 물론 제 생각엔 구글 딥마인드에서 오픈 소스로 풀어 버릴 수도 있습니다...

관련 리뷰 :  [https://techstory.shma.so/alphago-f2b8148c44e](https://techstory.shma.so/alphago-f2b8148c44e)

#### <span style="color: #808080;">neural-doodle</span>

![](https://github.com/alexjc/neural-doodle/raw/master/docs/Workflow.gif)

이 gif 하나로 모든게 설명이 될 거 같습니다.

신경망 알고리즘을 이용해서 이미지를 프로세싱 해 주는 시도입니다.

설치는 python3를 통해 실행을 하고 제가 example을 따라 실행한 것은

![Coastline](http://devpools.kr/wordpress/wp-content/uploads/2016/03/Coastline.png)

이런 결과가 나옵니다. 실행은 간단한데 CPU를 통해서 돌릴때는 시간이 너무 오래 걸리는군요.(  MBA 2012 기준 )

#### <span style="color: #808080;">image-anologies</span>

위의 neural doodle 과 너무 비슷한 프로젝트 입니다. 한번 이미지를 통해 보시죠.

![](https://raw.githubusercontent.com/awentzonline/image-analogies/master/examples/images/image-analogy-explanation.jpg)

트럼프 미 대선후보를 이용한 이미지도 있습니다. 사실은 이게 더 ... 직관적입니다.

![](https://raw.githubusercontent.com/awentzonline/image-analogies/master/examples/images/trump-image-analogy.jpg)

python을 이용해 만들어져 있습니다. 제대로된 GPU 가 없다면 느릴 거라고 이야기 합니다. 근데, 약간 트럼프가 뭐랄까. 까임의 아이콘인 모양이죠?

링크 : [https://github.com/awentzonline/image-analogies](https://github.com/awentzonline/image-analogies)

#### <span style="color: #808080;">leaf</span>

![](https://cdn-images-1.medium.com/max/800/1*cpn7ohXQOWv-IFnGMYhCOw.png)

leaf는 Rust로 만들어진 머신 러닝 플랫폼입니다.

> 구글의 Tensorflow 보다 빠르다.

리뷰 보기 : [https://techstory.shma.so/leaf-f1be6cc386d0](https://techstory.shma.so/leaf-f1be6cc386d0)

# Front-End-Web

javascript 쪽 관련 프로젝트는 7개 정도가 올라와 있는데, 알파고는 파이썬 이라 제외하면 6개 로 압축할 수 있습니다.

#### <span style="color: #999999;">FreeCodeCamp</span>

뭐 이제 FreeCodeCamp를 모르시는 분은 없을 거라고 생각하는데요.

![](https://camo.githubusercontent.com/60c67cf9ac2db30d478d21755289c423e1f985c6/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f66726565636f646563616d702f776964652d736f6369616c2d62616e6e65722e706e67)

사이트에 들어가서 샘플을 해 본 분들은 아시겠지만 과정중에 github 페이지 좋아요를 누르게 되어 있습니다.  github growth hacking의 좋은 예라고 볼 수 있겠습니다.

> 개발자들 끼리 같이 공부할 수 있는 코드캠프도 전 세계적으로 진행하고 있습니다.한국도 서울에서 진행을 하고 있네요.
> 
> [https://www.facebook.com/groups/free.code.camp.seoul/](http://t.umblr.com/redirect?z=https%3A%2F%2Fwww.facebook.com%2Fgroups%2Ffree.code.camp.seoul%2F&amp;t=ODBmM2UzMGJjZDQ2MmQxYmU1MjBiNWU2N2VmNDVmNmQ2N2E0NjRmOCxWUmJjTENsMg%3D%3D)

리뷰 보기 : [http://tech.shma.so/post/138852047393/github-trendfreecodecamp](http://tech.shma.so/post/138852047393/github-trendfreecodecamp)

이전 리뷰 : [https://github.com/TeamSEGO/github-trend-kr/blob/master/021_201508-weekly/021-03-freecodecamp.md](https://github.com/TeamSEGO/github-trend-kr/blob/master/021_201508-weekly/021-03-freecodecamp.md)

#### 

#### <span style="color: #999999;">Google-Play-Music-Desktop-Player-UnOFFICIAL</span>

Google play music을 electron 기반의 데스크탑 애플리케이션으로 개발했네요. 사용성은 매우 훌륭합니다만, 공식 앱이 있는 마당에 어떤 부분이 더 좋은지는 의문(?)입니다.

![](https://cdn-images-1.medium.com/max/2000/1*Qh8YBo47zIkAaoOLz5Xuxw.png)

리뷰 : [https://techstory.shma.so/google-play-music-desktop-player-unofficial-fbcbb95909da](https://techstory.shma.so/google-play-music-desktop-player-unofficial-fbcbb95909da)

#### <span style="color: #999999;">parse-dashboard</span>

![](https://github.com/ParsePlatform/parse-server/raw/master/.github/parse-server-logo.png?raw=true)

한 번도 다뤄보지는 않았지만, parse 서비스가 종료를 예고 하면서 대체수단들이 많이 생기기 시작했습니다.  (2017년 1월 28일까지 서비스)

[parse server](http://parse.com/) 는 node.js/express.js 기반의 오픈소스 API 서버 입니다.

parse-dashboard는 standalone parse app  들의 dashboard  오픈 소스입니다.

링크 : [https://github.com/ParsePlatform/parse-dashboard](https://github.com/ParsePlatform/parse-dashboard)

ParsePlatform에서 직접 제공하니, 2017년이 오기 전에  standalone으로 parse 를 서비스 할 사람에게는 좋은 프로젝트입니다.

parse server 프로젝트 :  [https://github.com/ParsePlatform/parse-server](https://github.com/ParsePlatform/parse-server)

#### <span style="color: #999999;">Hilo</span>

Hilo는 Alibaba그룹에서 오픈소스 프로젝트로 내 놓은 HTML5용 Game Development 툴입니다.

![](https://camo.githubusercontent.com/2a776993b79b85a3981c59db0259d3a8fa11f2e7/68747470733a2f2f696d672e616c6963646e2e636f6d2f7470732f54423176446c424c565858585863445856585858585858585858582d3835302d3830362e706e67)

flappy bird, 2048 같은 눈에 익은 게임들도 HTML5컨버팅을 해 놓았네요.

링크 : [https://github.com/hiloteam/Hilo](https://github.com/hiloteam/Hilo)

#### <span style="color: #999999;">React-vis</span>

![](https://i.imgur.com/IhHrIqV.png)

uber  에서 내 놓은 react visulaization 툴입니다.

아직 차트 자체는 일반적이긴 한데, 한번 두고 볼만은 해 보입니다.

링크 : [https://github.com/uber-common/react-vis](https://github.com/uber-common/react-vis)

#### <span style="color: #999999;">hamburgers</span>

hamburger UX는 UX쪽에서도 효용성에 대해서 왈가왈부가 많습니다. 하지만 관련 프로젝트는 왈가왈부할 필요가 없겠지요

![](https://camo.githubusercontent.com/e31deea6ce94f9dfe4fc2dd8b39fa6b4f8913aa7/687474703a2f2f692e696d6775722e636f6d2f743763556a44752e676966)

프로젝트 링크 : [https://github.com/jonsuh/hamburgers](https://github.com/jonsuh/hamburgers)

#### <span style="color: #808080;">baloon.css</span>

순수 css 만으로 툴팁을 구성했습니다.

![](https://github.com/kazzkiq/balloon.css/raw/master/sample.gif)

링크 : [https://github.com/kazzkiq/balloon.css](https://github.com/kazzkiq/balloon.css)

#### <span style="color: #808080;">rebass</span>

![](https://cdn-images-1.medium.com/max/2000/1*2_4nz277BgjkpQIUIuzYEA.png)

며칠전 react-how-to 를 링크건 적이 있었는데요. 거기에서는 inline style 은 아주 실험적인(bleeding-edge) 기술이라고 표현했고, React자체와 ES6 같은 것들에 익숙해 지면 도전해 보라고 했습니다. (관련 리뷰 :  https://github.com/ehrudxo/react-howto/blob/master/README-ko.md )

거기서 언급한 인라인 스타일을 쉽게 해 줄 수 있는 프로젝트입니다.

링크 : [https://github.com/jxnblk/rebass](https://github.com/jxnblk/rebass)

리뷰 : [https://techstory.shma.so/rebass-7e8baaec1bda](https://techstory.shma.so/rebass-7e8baaec1bda)

&nbsp;

단순한 인라인 스타일 적용을 떠나서 bootstrap 정도의 완성도를 지향하고 있는 것 같은 대단한(?) 프로젝트 입니다.

&nbsp;