---
title: 헤드리스 크롬과 selenium2의 조합을 사용해 보자 with node
id: 2539
comment: false
categories:
  - 'web test, test suite, headless chrome, selenium'
date: 2017-06-07 13:31:34
tags:
---

최근에 headless chrome 이 나름 화제가 되어서 돌았는데 이 headless 라는 의미가 어떻게 쓰이는지 잘 모르는 사람들을 위해 이야기를 하고 넘어갈까 한다.

## [](http://keen.devpools.kr/2017/06/07/about-test/#headless-browser "headless browser")headless browser

headless browser는 기본적으로 GUI 없는 웹 브라우저를 의미한다.

> “A headless browser is a web browser without a graphical user interface.”
> 출처 : [What is a headless browser?](http://blog.arhg.net/2009/10/what-is-headless-browser.html)

즉 CLI(Command Line interface)에서만 다루는 브라우저를 이야기 한다.유명한 헤드리스 브라우저로는  phantomJS 가 있다.

헤드리스 브라우저가 사용되는 예는 여러가지가 있는데 좋은 예로는 테스트 자동화를 할 수 있고 데이타를 긁어오기(scraping) 하는 데 사용되고 스크린샷을 뜨는데에도 손쉽게 사용된다. 웹페이지 반응을 자동으로 스크립팅할 수 있는 부분도 존재한다.나쁜 예로는 DDOS 공격을 하는데 사용되기도 하고, 자동화를 좋지 않은데에 쓰이기도 한다는 것이다.

## [](http://keen.devpools.kr/2017/06/07/about-test/#selenium%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8B%A4%EB%A5%B8%EA%B0%80 "selenium은 어떻게 다른가")selenium은 어떻게 다른가

셀레니엄은 태생 자체가 다르다고 보면 된다. 헤드리스 브라우저는 범용적인 목적에 따라 CLI환경에서 브라우저 환경을 에뮬레이션 하는 것이라고 하면 selenium은 브라우저 플러그인을 넣고 테스트를 실행시킨다. 서버 사이드에서 테스트에 관련된 실행을 시킬 수 있는 리모트 컨트롤러가 존재하고 다양한 브라우저를 지원하기 위해 드라이버들을 제공하는데 webdriver 라고 불려진다. 이후 버전이 업데이트 되었다.

클라이언트 서버 구조로 서버 사이드와 RC(Remote Control)로 구성되어 있던 것을 webdriver와 결합하면서 현재의 selenium2가 된 것이다.

![selenium1 + webdriver = selenium2](http://keen.devpools.kr/images/seleniumhq.jpg)

즉 CLI 툴로 사용할 수 있는 헤드리스 크롬의 경우는 다양한 브라우저를 테스트의 목적으로 사용해야 하는 범용 테스트 목적 보다는 다른 용도로 많이 사용될 것으로 보인다. <del>DDOS machine?</del>

## [](http://keen.devpools.kr/2017/06/07/about-test/#%EC%9E%90-%EA%B7%B8%EB%9F%AC%EB%A9%B4-node-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-selenium2%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4-%EB%B3%B4%EC%9E%90 "자 그러면 node 환경에서 selenium2를 사용해 보자.")자 그러면 node 환경에서 selenium2를 사용해 보자.

nightwatch 혹은 webdriverio는 node 환경에서 selenium2를 사용할 수 있게 해 준다. 옵션과 홈페이지, 구글 트렌드를 생각하면 nightwatch를 이용해야겠지만 일단 간단하게 사용하기 위해 webdriver로 실행을 해 보자.(robotframework도 같이 고려)

전역 옵션으로 webdriverio를 아래와 같이 설치한다.(nightwatch의 경우도 같이 진행할 수 있음.)
<figure class="highlight plain">

<table><tr><td class="gutter"><pre><div class="line">1</div></pre></td><td class="code"><pre><div class="line">&gt;npm install -g webdriverio</div></pre></td></tr></table>

</figure>

selenium2는 다음과 같은 명령으로 내려받을 수 있다.
<figure class="highlight plain">

<table><tr><td class="gutter"><pre><div class="line">1</div></pre></td><td class="code"><pre><div class="line">&gt;curl -O http://selenium-release.storage.googleapis.com/3.0/selenium-server-standalone-3.4.0.jar</div></pre></td></tr></table>

</figure>

크롬용 웹드라이버 -chromedriver를 받아서 압축을 풀고 PATH에 적용 시켜 준다.
<figure class="highlight plain">

<table><tr><td class="gutter"><pre><div class="line">1</div></pre></td><td class="code"><pre><div class="line">&gt;curl -O https://chromedriver.storage.googleapis.com/index.html?path=2.29/chromedriver_mac64.zip</div></pre></td></tr></table>

</figure>

## [](http://keen.devpools.kr/2017/06/07/about-test/#headless-chrome-%EC%9D%84-%EC%84%A4%EC%B9%98-%ED%95%9C%EB%8B%A4 "headless chrome 을 설치 한다.")headless chrome 을 설치 한다.

이 경우는 며칠전만 해도 canary를 쓴다고 했지만 지금은 크롬 최신버전이면 다음의 옵션만으로 실행할 수 있다.(MacOS 의 경우)
<figure class="highlight plain">

<table><tr><td class="gutter"><pre><div class="line">1</div></pre></td><td class="code"><pre><div class="line">&gt; chrome --headless --disable-gpu --screenshot &quot;http://devpools.kr&quot;</div></pre></td></tr></table>

</figure>

## [](http://keen.devpools.kr/2017/06/07/about-test/#%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1 "테스트 코드 작성")테스트 코드 작성

아래와 같이 테스트 코드를 작성하고 나면 일단은 selenium2 기반의 테스트 프레임워크의 시작을 했다고 보면 된다.
<figure class="highlight javascript">

<table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div><div class="line">10</div><div class="line">11</div><div class="line">12</div><div class="line">13</div><div class="line">14</div><div class="line">15</div><div class="line">16</div><div class="line">17</div><div class="line">18</div><div class="line">19</div><div class="line">20</div><div class="line">21</div></pre></td><td class="code"><pre><div class="line"><span class="comment">//test.js</span></div><div class="line"><span class="keyword">var</span> webdriverio = <span class="built_in">require</span>(<span class="string">'webdriverio'</span>);</div><div class="line"><span class="keyword">var</span> options = &#123;</div><div class="line">    <span class="attr">desiredCapabilities</span>: &#123;</div><div class="line">        <span class="attr">browserName</span>: <span class="string">'chrome'</span>,</div><div class="line">        <span class="attr">chromeOptions</span>: &#123;</div><div class="line">            <span class="attr">args</span>: [</div><div class="line">                   <span class="string">'headless'</span>,</div><div class="line">                   <span class="string">'disable-gpu'</span>,</div><div class="line">               ],</div><div class="line">        &#125;</div><div class="line">    &#125;</div><div class="line">&#125;;</div><div class="line">webdriverio</div><div class="line">    .remote(options)</div><div class="line">    .init()</div><div class="line">    .url(<span class="string">'http://www.devpools.kr'</span>)</div><div class="line">    .getTitle().then(<span class="function"><span class="keyword">function</span>(<span class="params">title</span>) </span>&#123;</div><div class="line">        <span class="built_in">console</span>.log(<span class="string">'Title was: '</span> + title);</div><div class="line">    &#125;)</div><div class="line">    .end();</div></pre></td></tr></table>

</figure>
결과는 다음과 같다.

![개발바보들의 타이틀을 긁어왔다](http://keen.devpools.kr/images/headlessscraping.jpg)

해당 소스는 깃헙의 다음 링크에서 받아볼 수 있다.
[https://github.com/ehrudxo/headlesssample](https://github.com/ehrudxo/headlesssample)

.