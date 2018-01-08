---
title: 4월 1주차 깃헙트렌드 Java편.
tags:
  - Android
  - Java
  - UI
id: 269
categories:
  - GitHub-trend
date: 2016-04-04 19:59:03
---

안녕하세요. 금주에는 깃헙트렌드중 Java <del>라 쓰고 Android 라고 읽는다</del> 에 올라온 핫한 Top 5를 소개시켜 드리려고 합니다.

&nbsp;

[1\. StatusBarUtil](https://github.com/laobie/StatusBarUtil)

![](https://github.com/laobie/StatusBarUtil/raw/master/img/set_color.png)

Android 앱 UI의 완성도를 위해 건드려야할 요소가 여러가지가 있는데 그중에 은근히 건드리기 귀찮은 StatusBar의 색상이나 투명도를 설정할 수 있는 Util입니다.

<pre><span class="pl-smi">StatusBarUtil</span><span class="pl-k">.</span>setColor(<span class="pl-smi">Activity</span> activity, <span class="pl-k">int</span> color);</pre>

<pre><span class="pl-smi">StatusBarUtil</span><span class="pl-k">.</span>setTranslucent(<span class="pl-smi">Activity</span> activity, <span class="pl-k">int</span> statusBarAlpha)</pre>

코드에서 어떻게 사용하는지 느낌이 확 오네요. API19+에서 사용가능합니다.

&nbsp;

[2\. WelcomeCoordinator](https://github.com/txusballesteros/welcome-coordinator)

![](https://github.com/txusballesteros/welcome-coordinator/raw/master/assets/onboarding_demo.gif)![](https://github.com/txusballesteros/welcome-coordinator/raw/master/assets/welcome_demo.gif)

앱의 웰컴페이지를 손쉽게 만들수 있도록 도와주는 페이지입니다. 앱 최초 실행시 또는 인트로에 사용되는 여러 Layout을 묶어서 페이지의 이동을 편하게 도와줍니다. 오늘 소개시켜드리는 대부분의 자료들이 UI라서 직접 보는 것 만큼 좋은 설명은 필요없어 보입니다.

&nbsp;

[3\. VideoListPlayer](https://github.com/waynell/VideoListPlayer)

![](https://github.com/waynell/VideoListPlayer/raw/master/art/preview.gif)

ListView 또는 RecyclerView에 재생을 컨트롤할 수 있는 요소를 추가시켜 마치 instagram의 List를 스크롤할때의 경험을 제공합니다. 최근 trend상위에 중국 개발자의 작품이 많이 올라오는데 이것 역시 그들의 작품이네요. 하지만 우리는 code로 communication하니까...

&nbsp;

[4\. AndroidSpinKit](https://github.com/ybq/Android-SpinKit)

![](https://raw.githubusercontent.com/ybq/AndroidSpinKit/master/art/screen.gif)

기하학적인 도형들의 디자인으로 구성된 Android Native UI Components set입니다. Button / TextView / ImageView /ProgressBar로 구성되어 있으며 그간 Material Design이 좀 지겨워지는 유저를 위한 선택이 될수 있겠네요. 이것도 역시 중국 개발자의 작품으로써 중국어를 배워야하는 것이 아닌가 하는 걱정도 슬슬 들기 시작합니다. <del>하지만 우리에겐 구글 번역기가 있기에...</del>

&nbsp;

[5\. BoomMenu](https://github.com/Nightonke/BoomMenu)

![](https://raw.githubusercontent.com/Nightonke/BoomMenu/master/Pictures/show_circle.gif)![](https://raw.githubusercontent.com/Nightonke/BoomMenu/master/Pictures/show_ham.gif)

이번주 마지막으로 소개시켜드릴 내용도 역시 Android UI네요. BoomMenu라는 UI Components인데, Hamberger Button, Action Bar Menu, Floating Action Button 등을 화려하게 변신시켜 줍니다. 가이드를 아주 잘 만들어 놨네요. 부담없이 사용하셔도 될듯합니다.