---
title: '[JavaScript] ESLint with WebStorm'
tags:
  - ESLint
  - IntellisJ
  - WebStorm
id: 696
categories:
  - JavaScript
date: 2017-02-16 19:03:54
---

IntelliJ 의 웹 버전인 WebStorm을 사용할 때 ESLint를 사용하는 방법을 알아보겠습니다. 프로젝트에도 적용할 수 있겠지만 에디터에서 자동으로 문제가 있는 (혹은 버그를 생산하는) 코드를 미리 찾아 볼 수 있습니다.

### ESLint

"Linting"이라는 행위는 버그가 날 수 있을 만한 코드를 찾아서 체크를 해 주는일을 의미합니다.

> In [computer programming](https://en.wikipedia.org/wiki/Computer_programming "Computer programming"), **lint** is a Unix utility that flags some [suspicious and non-portable constructs](https://en.wikipedia.org/wiki/C_(programming_language)#Language_tools "C (programming language)") (likely to be bugs) in [C language](https://en.wikipedia.org/wiki/C_(programming_language) "C (programming language)")[source code](https://en.wikipedia.org/wiki/Source_code "Source code"); generically, lint or a **linter** is any tool that flags suspicious usage in software written in any [computer language](https://en.wikipedia.org/wiki/Computer_language "Computer language"). The term **lint-like behavior** is sometimes applied to the process of flagging suspicious language usage. Lint-like tools generally perform [static analysis](https://en.wikipedia.org/wiki/Static_code_analysis "Static code analysis") of source code. - 위키피디아 [Lint](https://en.wikipedia.org/wiki/Lint_%28software%29)

JavaScript 에서 이 Lint라는 의미를 처음 얘기한 사람은 Douglas crockford 였고 그는 JsLint라는 툴을 만들어 냅니다. JavaScript Definite Guide  같은 책에서도 내용을 언급하고 있습니다.

[contentcards url="http://www.jslint.com/" target="_blank"]

하지만 최근의 대부분의 JavaScript 커뮤니티에서 사용하는 Lint  툴은 ESLint 툴입니다.

Nicholas Jakas에 의해 2013년에 나온 이 툴은 현재 여러 툴들에서 플러그인으로 사용되고 있습니다.

[contentcards url="http://eslint.org/" target="_blank"]

"JSX" 지원하는 것에 대해서 언급이 첫페이지에 있습니다.  React 와 JSX는 다르다고 하는군요.  아무래도 Pluggable한 아키텍처로 이루어져 있기에 오리지날인 JSLint에 비해 많이 쓰이는 거라고 보여집니다.

### webstorm

웹스톰은 IntelliJ를 만든 Jetbrains에서 만든 웹 개발자용 IDE 입니다. 이클립스가 약간은 범용에 가깝다고 하면  이 웹 스톰은 JavaScript 최근 트렌드 및 개발 환경에 대한 이해가 큰 IDE 입니다. Node 모듈에 대한 이해도 가지고 있고 npm  을 기본적으로 작동시킬 수 있습니다. 유료 IDE기는 하지만 IntelliJ에 적응된 개발자들은 이webstorm의 여러가지 기능에 대해서 많이 만족하고 쓰고 있습니다. 최근 팝잇에 올라온 IntelliJ에 관한 글을 읽어보면 많은 인사이트들을 얻을 수 있습니다.

[contentcards url="http://www.popit.kr/%EC%9D%B8%ED%85%94%EB%A6%ACj-%ED%99%9C%EC%9A%A9-%EA%BF%80%ED%8C%81-42%EA%B0%80%EC%A7%80-%EC%A0%95%EB%A6%AC/" target="_blank"]

### WebStrom + ESLint

이 WebStorm에서 ESLint를 설정하는 방법은 다음과 같습니다.

preference -&gt; Languges &amp; Framework -&gt; ESLint

![](/images/2017/02/20170306_061320-1024x712.jpg)

두가지를 셋팅해 주셔야 하는데 하나는 Node 를 지정하는 것이고 또 다른 하나는 ESLint 폴더를 지정해 주는 것입니다. Node 는 각자의 환경을 정하면 될 것입니다.

ESLint 폴더를 지정할 때는

<pre><span style="font-size: 12pt; font-family: terminal, monaco, monospace;">$ npm list -g|more </span></pre>

옵션을 통해서 어디에 설치를 했는지 확인해 주시면 됩니다.

확인이 되셨으면

<pre><span style="font-size: 12pt; font-family: terminal, monaco, monospace;">`$ npm install -g eslint`</span></pre>

[contentcards url="https://github.com/eslint/eslint" target="_blank"]

이후 몇가지 모듈을 더 설치해야 합니다.

<pre><span style="font-family: terminal, monaco, monospace; font-size: 12pt;">$npm install eslint-plugin-react eslint-plugin-jsx-a11y eslint-plugin-import -g</span></pre>

실행해 주고 나서 WebStorm을 다시 작동시키면 아래와 같은 좋은 예를 볼 수 있습니다.

![](/images/2017/02/스크린샷-2017-02-16-오후-6.49.10.jpg)

&nbsp;