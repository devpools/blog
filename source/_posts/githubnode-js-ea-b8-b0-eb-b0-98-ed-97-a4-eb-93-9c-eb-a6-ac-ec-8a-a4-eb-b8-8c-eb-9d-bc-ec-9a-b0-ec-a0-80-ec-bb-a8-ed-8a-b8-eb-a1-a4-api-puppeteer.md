---
title: '[github]Node.js 기반 헤드리스 브라우저 컨트롤 API - puppeteer'
id: 4521
comment: false
categories:
  - 'github-trend, github'
date: 2017-09-14 13:09:55
tags:
---

![puppeteer](http://keen.devpools.kr/images/puppeteer.jpg)
프로젝트 링크 : [https://github.com/GoogleChrome/puppeteer/](https://github.com/GoogleChrome/puppeteer/)
첫번째 프로젝트는 구글 크롬 프로젝트에서 Headless Chrome Node API 로 내 놓은 퍼펫티어다. 퍼펫이라고 하면 번역하면 인형술사이고 꼭두각시 인형을 다루는 사람을 이야기 한다. 이 프로젝트의 배경부터 이야기 하자면 올해(2017년) 4월 경 구글프로젝트에서는 크롬에 Headless 모드를 추가했다.
Headless 브라우저라고 하면 일반적으로 CLI환경에서 브라우저가 시각적으로 보여지지 않고 백그라운드에서 작동할 수 있는 것을 이야기 하는데, 이 전까지 가장 유명한 것은 PhantomJS라는 프로젝트였다. 크롤링을 하고 스크린 샷을 찍어서 다운로드하고 등의 일들을 할 수 있었다. 그래서 데스크탑이 아닌 서버에서 DOM을 읽어야 하는 경우 등에서 많이 사용되고는 했었다.

필자의 경우는 테스트 환경을 꾸밀 수 있지 않을까 하는 기대에 관련된 작업들을 진행해 보았고 관련되어 링크를 남기기도 했다.
링크 : 헤드리스 크롬과 selenium2의 조합을 사용해 보자 with node ([http://keen.devpools.kr/2017/06/07/about-test/](http://keen.devpools.kr/2017/06/07/about-test/) )

그런데, 8월 한달 가장 주목받은 프로젝트가 된 이 puppeteer는 Node.js에서 헤드리스 크롬을 사용할 수 있는 API들을 제공하는 것이다. Headless 모드를 발표하자마자 Phantom.JS는 더 이상 개발 안하기로 선언을 한 것과 마찬가지로 Node.js 진영에 새로운 무기가 생겨버린 셈이 되었다

## [](http://keen.devpools.kr/2017/09/14/puppeteer/#1-%EC%84%A4%EC%B9%98%EB%A5%BC-%ED%95%B4%EB%B3%B4%EC%9E%90 "1\. 설치를 해보자")1\. 설치를 해보자

먼저 프로젝트를 한번 만들어 보자.
<figure class="highlight bash">

<table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div></pre></td><td class="code"><pre><div class="line"><span class="variable">$mkdir</span> puppeteer-project &amp;&amp; <span class="built_in">cd</span> puppeteer-project</div><div class="line"><span class="variable">$yarn</span> add puppeteer</div></pre></td></tr></table>

</figure>

이렇게 설치를 하고 나면 프로젝트에 index.js 파일을 만든다..

&lt;코드&gt;index.js
<figure class="highlight javascript">

<table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div><div class="line">10</div></pre></td><td class="code"><pre><div class="line"><span class="keyword">const</span> puppeteer = <span class="built_in">require</span>(<span class="string">'puppeteer'</span>);</div><div class="line"></div><div class="line">(<span class="keyword">async</span> () =&gt; &#123;</div><div class="line">  <span class="keyword">const</span> browser = <span class="keyword">await</span> puppeteer.launch();</div><div class="line">  <span class="keyword">const</span> page = <span class="keyword">await</span> browser.newPage();</div><div class="line">  <span class="keyword">await</span> page.goto(<span class="string">'http://devpools.kr'</span>);</div><div class="line">  <span class="keyword">await</span> page.screenshot(&#123;<span class="attr">path</span>: <span class="string">'devpools.png'</span>&#125;);</div><div class="line"></div><div class="line">  browser.close();</div><div class="line">&#125;)();</div></pre></td></tr></table>

</figure>

스크린샷을 가져 오는 코드가 작성되었다.  개발바보들 첫 페이지의 스크린 샷을 가져오는 소스 코드를 작성한 이후에 node index.js 명령어를 입력하면 다음과 같은 이미지를 가지고 오는 것을 볼 수 있다.

기본적으로 지정된 이미지 크기는 800*600으로 지정된 듯 하다. 보통 phantomJS 같은 경우는 스크롤을 다 잡아 가던 초반 모습에 비해 메모리 관리를 위한 것인지 이미지 해상도도 그렇게 좋은 거 같지는 않아 보인다.

PDF로 Export 하는 기능도 API를 통해 구현이 가능하다.

![개발바보들 스크린샷](http://keen.devpools.kr/images/crawled_devpools.jpg)

## [](http://keen.devpools.kr/2017/09/14/puppeteer/#2-%ED%81%AC%EB%A1%A4%EB%A7%81%EC%9D%84-%ED%95%B4-%EB%B3%B4%EC%9E%90 "2\. 크롤링을 해 보자.")2\. 크롤링을 해 보자.

이번엔 find.js 라는 파일을 아래와 같이 만들어 본다. 실제로 이미지와 지금 아래 크롤링 소스는 해당 프로젝트와 내용이 거의 유사하다. 
<figure class="highlight javascript">

<table><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div><div class="line">10</div><div class="line">11</div><div class="line">12</div><div class="line">13</div><div class="line">14</div><div class="line">15</div><div class="line">16</div><div class="line">17</div><div class="line">18</div><div class="line">19</div><div class="line">20</div><div class="line">21</div><div class="line">22</div><div class="line">23</div><div class="line">24</div><div class="line">25</div><div class="line">26</div><div class="line">27</div></pre></td><td class="code"><pre><div class="line"><span class="meta">'use strict'</span>;</div><div class="line"></div><div class="line"><span class="keyword">const</span> puppeteer = <span class="built_in">require</span>(<span class="string">'puppeteer'</span>);</div><div class="line"></div><div class="line">(<span class="keyword">async</span>() =&gt; &#123;</div><div class="line"><span class="comment">//브라우저 객체와 페이지 객체를 만들고</span></div><div class="line"><span class="keyword">const</span> browser = <span class="keyword">await</span> puppeteer.launch();</div><div class="line"><span class="keyword">const</span> page = <span class="keyword">await</span> browser.newPage();</div><div class="line"></div><div class="line"><span class="comment">// 구글 검색창 방문</span></div><div class="line"><span class="keyword">await</span> page.goto(<span class="string">'https://google.com'</span>, &#123;<span class="attr">waitUntil</span>: <span class="string">'networkidle'</span>&#125;);</div><div class="line"><span class="comment">//개발 바보들에 대한 검색어를 입력하고</span></div><div class="line"><span class="keyword">await</span> page.type(<span class="string">'devpools'</span>);</div><div class="line"><span class="comment">// 검색버튼을 누르고</span></div><div class="line"><span class="keyword">await</span> page.click(<span class="string">'input[type="submit"]'</span>);</div><div class="line"><span class="comment">//검색한 결과의 타이틀을 가져와서</span></div><div class="line"><span class="comment">// Wait for the results to show up</span></div><div class="line"><span class="keyword">await</span> page.waitForSelector(<span class="string">'h3 a'</span>);</div><div class="line"></div><div class="line"><span class="keyword">const</span> links = <span class="keyword">await</span> page.evaluate(<span class="function"><span class="params">()</span> =&gt;</span> &#123;</div><div class="line">  <span class="keyword">const</span> anchors = <span class="built_in">Array</span>.from(<span class="built_in">document</span>.querySelectorAll(<span class="string">'h3 a'</span>));</div><div class="line">  <span class="keyword">return</span> anchors.map(<span class="function"><span class="params">anchor</span> =&gt;</span> anchor.textContent);</div><div class="line">&#125;);</div><div class="line"><span class="comment">//콘솔에 출력한다.</span></div><div class="line"><span class="built_in">console</span>.log(links.join(<span class="string">'\n'</span>));</div><div class="line">browser.close();</div><div class="line">&#125;)();</div></pre></td></tr></table>

</figure>
소스 코드에 대한 설명은 주석에 달아 둔데로 브라우저 객체와 페이지 객체를 만들고 구글 검색창을 방문해서 개발 바보들에 대한 검색어를 입력하고 검색버튼을 누른 뒤 검색된 결과의 타이틀을 가져와서 콘솔에 출력한다.

그 결과는 다음과 같다. 이 과정 중에 어떤 브라우저의 인터렉션도 필요 없었고 (내부적으로는 크롬 헤드리스 브라우저가 작동을 했지만) 사용자의 경우는 결과만 가져올 수 있다.

![devpools 검색결과](http://keen.devpools.kr/images/searchresults.jpg)

## [](http://keen.devpools.kr/2017/09/14/puppeteer/#%EB%A7%BA%EC%9D%8C%EB%A7%90 "맺음말")맺음말

왜 구글은 이런 제품을 내놓고 있는 걸까? 워낙 혁신적인 기업이라 속내를 다 살펴볼 수는 없지만 지속적으로 API를 내놓고 있는 것은 웹의 많은 부분이 자동화로 돌아설 것이고 그 중심에 인공지능이 있지 않을까 하는 생각이 들어 잠시 한번 고민을 해 보았다. 아마도 텐서플로가 조만간 DOM 기반의 러닝 모델을 공개하는 날이 오지 않을까?