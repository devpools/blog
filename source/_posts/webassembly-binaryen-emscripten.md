---
title: WebAssembly - hello world 어셈블리를 브라우저에 올려보자
tags:
  - Asm.js
  - binaryen
  - Emscripten
  - WebAssembly
id: 428
categories:
  - GitHub-trend
date: 2017-01-21 22:07:01
---

WebAssembly 이름만 들어도 긴장되는 이 프로젝트는 지금 읽으시면서 생각하시는 그대로 web +  Assmbly의 조합입니다.

링크 : http://webassembly.org/

![스크린샷 2017-01-21 오후 10.11.16](http://www.popit.kr/wp-content/uploads/2017/01/스크린샷-2017-01-21-오후-10.11.16-569x600.jpg)

> **어셈블리어(assembly)**는 기계어와 일대일 대응이 되는 컴퓨터 프로그래밍의 저급 언어이다.
> 
> 출처: [http://blog.opid.kr/162](http://blog.opid.kr/162) [opid's document]

브라우저 상에서 돌아가는 기계어라니 생각만 해도 즐거운 이 상상은 사실 비슷한 일들을 시도 했었던 몇 군데의 의기투합으로 완성됩니다.

&nbsp;

결론부터 얘기하자면 오늘 소개하고 싶은 최종 프로젝트는 Web Assmebly의 binaryen 입니다.

[contentcards url="https://github.com/WebAssembly/binaryen" target="_blank"]

이 프로젝트를 설명을 하기 위해서는 먼길을 가야 할 거 같습니다. 하지만 매우 흥미로운 주제라고 생각되니까 시작해 보겠습니다.

# **1\. asm.js 와 emscripten**

최근 몇년동안 브라우저에서 mame게임이라던지 둠이라던지 등의 오픈소스로 풀려있는 게임들에 대해서 브라우저에서 돌아가는 프로젝트를 많이들 보셨을 겁니다. 어제 저는 Quake3 를 웹에서 돌려 보았습니다.

[caption id="" align="aligncenter" width="600"]![크롬에서 퀘이크 하는 모습](https://pbs.twimg.com/media/A9Orz7pCUAAOU3a.png "브라우저에서 퀘이크") [그림1]크롬 퀘이크 출처 - https://twitter.com/search?q=%23quakejs 오오오 다 부시고 싶다!!!![/caption]먼저 브라우저 상에서 저런 게임 개발 + 연산을 하기 위해서는 어떤 일이 있어야 하는지를 생각해 봅시다.

1.  엄청나게 빠른 자바스크립트를 개발하는거야!
2.  네이티브 코드를 브라우저에서 돌리는 거야
3.  <del>WebGL로 짜면 안되나요?</del>

이렇게 접근한 눈에 띄는 큰 세력이 있었습니다. 하나는 asm.js라는 커뮤니티고 또 하나는 Emscripten 입니다.

#### 1.1\. asm.js.

링크 : http://asmjs.org/

![스크린샷 2017-01-21 오후 10.16.19](http://www.popit.kr/wp-content/uploads/2017/01/스크린샷-2017-01-21-오후-10.16.19-600x251.jpg)

asm.js는 다들 오해하시고 있는 부분이 있는데 어떤 새로운 컴파일 된 형태의 언어를 뜻하는 것은 아닙니다. JavaScript 스펙 중에 C나, C++ 처럼 좀더 low level로 건드릴 수 있는 형태로 나와 있는 API들을 사용해서 조금 덜 human readable한 코드지만 성능이 빠른 형태의 코드를 뜻합니다.

좀더 깊은 이해를 위해서는 아웃사이더 님의 다음글을 추천합니다.

[contentcards url="https://blog.outsider.ne.kr/927" target="_blank"]

어쨌든 결론만 놓고 보면  asm.js를 사용하면 native 코드에 비해 1.5배밖에 안느린 코드를 짤 수 있다. 라는 놀라운 일이 벌어집니다. GUI 쪽은 Java보다 대 놓고 빨라질 가능성을 가지게 되는 점입니다.

#### 1.2.Emscripten

이번엔 그렇다면 다른 방법으로 native 코드를 JavaScript로 바꿔야지 하고 접근한 프로젝트를 소개합니다.

​![스크린샷 2017-01-21 오후 10.19.21](http://www.popit.kr/wp-content/uploads/2017/01/스크린샷-2017-01-21-오후-10.19.21-600x165.jpg)

사이트에가서 보면 아시겠지만 이 프로젝트는 LLVM(Low Level Virtual Machine) 기반으로 C,C++로 짜여진 것을 JavaScript로 바꿔주는 프로젝트라는 것을 알 수 있습니다.

#### 1.3\. 도원결의

[caption id="attachment_485" align="aligncenter" width="588"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/01/6Y3VF96RTIPAXWR2YQMH-1024x576.jpg) 출처 : 나무위키 진삼국무쌍7[/caption]

뭐 여러분 모두 짐작하셨다시피 asm.js의 엄청난 속도를 보고 emscripten은 LLVM을 asm.js로 컴파일 하기에 이릅니다.

1.4\. 한방 그림

[caption id="" align="alignnone" width="640"]![](http://i2.wp.com/i.imgur.com/1yoy1Fal.png) 출처 : http://ejohn.org/blog/asmjs-javascript-compile-target/[/caption]

그림은 jquery의 구루 john resig의 [사이트에](http://ejohn.org/blog/asmjs-javascript-compile-target/) 올라와 있는 그림인데, C/C++ 코드를 짜면 Clang이 LLVM의 바이트 코드로 뱉어 냅니다. 그 결과는 spec 이 있으므로 컴파일러가 이해하는 수준의 순서로 Asm.js 스펙에 넣고 OpenGL을 쓰는 경우는 WebGL 스펙까지도 내려가서 변환해 줍니다. <del>그림 하나면 이해가 될 거를 많이도 내려 왔습니다.</del>

# 2\. WebAssembly

드디어 오늘 이야기 할 WebAssembly 입니다. 자 이쯤 오면 이런 생각할 사람이 있을 거 같습니다.

> _**흠... 그럴게 아니라 진짜 Assembly Spec을 만드는게 어때?**_

<del>그런데 그것이 실제로 일어났습니다.</del>

어느 순간에 깃헙에 WebAssembly라는 그룹이 생기더니 design이라는 프로젝트를 통해 스펙을 주고 받고 있었습니다.

[contentcards url="https://github.com/WebAssembly/design" target="_blank"]

한 1년 정도 watch를 하고 두고 보고 있었습니다. 최근에 생긴변화 마일 스톤이 생겨서 한번 확인해 보면 좋겠다 싶어서 들여다 보았더니 생각보다 이야기 할 것들이 있습니다.

## Past Milestones

*   April 2015 - [WebAssembly Community Group](https://www.w3.org/community/webassembly) started
*   June 2015 - The first [public announcement](https://github.com/WebAssembly/design/issues/150) [[1](https://blogs.msdn.microsoft.com/mikeholman/2015/06/17/working-on-the-future-of-compile-to-web-applications/)][[2](https://blog.mozilla.org/luke/2015/06/17/webassembly/)]
*   March 2016 - Definition of core feature with multiple interoperable implementations [[1](https://blogs.windows.com/msedgedev/2016/03/15/previewing-webassembly-experiments)] [[2](https://v8project.blogspot.com/2016/03/experimental-support-for-webassembly.html)] [[3](https://hacks.mozilla.org/2016/03/a-webassembly-milestone/)]
*   October 2016 - Browser Preview announced with multiple interoperable implementations [[1](https://blogs.windows.com/msedgedev/2016/10/31/webassembly-browser-preview/)] [[2](http://v8project.blogspot.com/2016/10/webassembly-browser-preview.html)] [[3](https://hacks.mozilla.org/2016/10/webassembly-browser-preview)]

16년 12월에 브라우저  프리뷰로 올라온 버전 위주로 이야기를 할까 합니다.

#### 2.1\. binaryen

[caption id="" align="aligncenter" width="889"]![](https://kripken.github.io/talks/Daenerys3.jpg) 실제로 targaryen 네이밍 컨벤션을 따랐다고 합니다. 헐. 덕.[/caption]

뭔가 왕좌의 게임에 나올 거 같은 프로젝트 이름인데 실제로 emscripten 과 라임을 맞추기 위해서 노력했고 네이밍 컨벤션은  그걸 따랐다고 깃헙 공식페이지에 announce하고 있습니다. <del>개발의 시작은 덕질</del>

먼저 시작하기 전에 이 어셈블리 (바이너리) 파일 포맷을 wasm 라고 정의했고 파일 확장자는 .wasm이라는 것만 알아두시면 됩니다. <del>저는 이걸 아는데 시간이 많이 걸렸어요. </del>이후 binayen은 굉장히 간단합니다.  브라우저에서 이제 wasm 파일을 인식하니 두가지 옵션이 남았죠.

1.  emscripten으로 컴파일 할때 wasm 파일을 만들어 준다.
2.  asm.js 파일을  wasm 파일로 바꿔준다.

#### 2.2. hello world를 찍어보자.

백문이 불여일타! 개발자라면 응당 한번 찍어봐야 합니다.

##### 2.2.1\. 선행 설치

아 가슴뛰는 순간이네요. 일단 emscripten 과 binaryen을 인스톨 해야죠. 여기서 상당히 헤메었엇는데 저만 믿고 따라오시면 됩니다.

먼저 EMSDK라는 Emscripten SDK를 설치 하셔야 됩니다. 아래 걸어둔 링크에 따라 다운로드 받고 순서대로 실행하시면 됩니다.

링크 : http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html

![스크린샷 2017-01-22 오후 1.46.23](http://www.popit.kr/wp-content/uploads/2017/01/스크린샷-2017-01-22-오후-1.46.23-600x405.jpg)

그런데 문제가 있습니다.  EMSKD의 명령어 중에 옵션에 latest 를 하면 1.35 버전을 가지고 오게 되서 binaryen을 지원하지 않습니다. 바꿔야 되는 부분은 제가 빨간색으로 표시해 두겠습니다.

<pre><span class="cp"># Fetch the latest registry of available tools.</span>
<span class="p">.</span><span class="o">/</span><span class="n">emsdk</span> <span class="n">update</span>

<span class="cp"># Download and install the latest SDK tools.</span>
<span class="p">.</span><span class="o">/</span><span class="n">emsdk</span> <span class="n">install</span> <span class="n" style="color: #ff0000;">incoming</span>

<span class="cp"># Make the "latest" SDK "active"</span>
./emsdk activate <span style="color: #ff0000;">sdk-incoming-64bit</span></pre>

&nbsp;

으로 변경을 해 주셔야만 제대로 컴파일이 됩니다.

먼저 helloworld.c 소스를 작성해 보겠습니다.

<pre>#include &lt;stdio.h&gt;

int main() {
 printf("hello, world!\n");
 return 0;
 }</pre>

이제 다음과 같은 명령어를 통해 컴파일을 실행하면

<pre>$emcc hello_world.c -o hello.js -s 'BINARYEN="~/dev/native-osx/binaryen"'</pre>

hello.wasm 파일이 떨어집니다.

hex  에디터로 열어서 hello, world를 찾아 보는것이 도리겠죠?

[caption id="attachment_436" align="aligncenter" width="998"]![](http://devpools.kr/wordpress/wp-content/uploads/2017/01/스크린샷-2017-01-21-오후-9.51.07.jpg) 오.... 소름[/caption]

그런데... 이걸 어떻게 브라우저에 띄울까요? 걱정 안해도 됩니다.  브라우저에서 볼 수 있도록 명령어를 제공합니다.

<pre>emcc hello_world.c -s WASM=1 -o hello.html</pre>

타겟을 html로 주고 WASM=1로 주게 되면 우리가 필요한 모든 파일들을 볼 수 있으며textarea 에 뜨는 것을 확인할 수 있습니다.![](http://devpools.kr/wordpress/wp-content/uploads/2017/01/스크린샷-2017-01-21-오후-9.55.09-1024x640.jpg)

물론 실행은 크롬 canary 버전에서 실행했습니다. 아직 프리뷰 버전이라 정식 버전에는 들어가지 않았습니다.

#### 2.3\. 한방 그림

**C,C++ -&gt; Clang -&gt; LLVM -&gt; WASM** 로 바로 되는 그림이면 좋겠지만 아직은  **C,C++ -&gt; (Clang -&gt; LLVM -&gt; asm.js )-&gt; WASM** 과정으로 이루어지는 형태입니다. 물론 이런 것들은 시간이 더 지나면서 바뀌게 될 것으로 보입니다. 괄호 안의 것들을 지우는 작업이 미래의 일이리라 보여집니다.

# **3\. 여담들**

https://www.youtube.com/watch?v=Ikbd7KrrAeM

1.  마지막으로 Angry bots 가 브라우저상에 돌아가는 동영상이고 저는 직접 플레이 해 보았는데 그냥.. 3D게임을 그대로 돌리는데 전혀 이상없는 수준이고 로딩 속도도 훨씬 빠르더군요.

이 프로젝트는 지속적으로 주목해 봐야 합니다. 단연코!

1.  참고로 WASM 파일을 어떻게 다룰 것인지는 아래 링크를 참조하시기 바랍니다.

링크 : http://webassembly.org/docs/js/

![스크린샷 2017-01-22 오후 1.49.16](http://www.popit.kr/wp-content/uploads/2017/01/스크린샷-2017-01-22-오후-1.49.16-600x370.jpg)

1.  이전에 emscripten같은 프로젝트가 없었냐 하면 그것도 아닙니다. GWT는 java를 JavaScript로 바꾸는 작업을 했었고 이런 일련의 과정들이 현재의 밑거름이 되겠죠.