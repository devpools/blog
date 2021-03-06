---
title: jekyll로 Github Page 만들기
id: 2578
comment: false
date: 2017-03-16 09:00:00
tags:
---

Markdown문서로 문서를 작성하고, 이를 자동으로 정적인 페이지로 변환시켜주는 [Jekyll](https://jekyllrb-ko.github.io/)을 사용하여 손쉬운 블로그를 만들어 보도록 합시다. 이에 Gitbub Page는 Jekyll을 통해 서비스가 가능한 최고의 선택이 됩니다. Jekyll과 Github Page의 조합은 많은이들에게 사용된지 상당히 오래되었으므로, 본 포스팅에서는 현재의 페이지가 어떤 과정으로 생성되었는지에 대한 과정을 서술합니다.

[송성광님의 포스트](http://blog.saltfactory.net/upgrade-github-pages-dependency-versions/)를 참고하여 초기에 세팅을 진행했습니다. Ruby를 베이스로 한 Jekyll을 사용하기 위해서는 다양한 설정이 필요한데, 위의 포스팅은 초보자도 어려움 없이 맥에서 Jekyll환경을 세팅하기 위한 아주 자세한 설명이 담겨있습니다. 작성글대로 한다면 큰 어려움 없이 Github Page에 글을 작성하기 위한 준비를 하실 수 있을 것입니다.

Jekyll은 전세계 개발자들이 미리 만들어 놓은 좋은 테마를 무료(또는 유료?)로 사용이 가능합니다. [themes.jekyllrc.org](http://themes.jekyllrc.org/), [jekyllthemes.org](http://jekyllthemes.org/) 등을 통하여 본인의 취향에 맞는 테마를 선택하도록 합시다. 글쓴이는 [holo-alfa](http://steinvc.github.io/holo-alfa/)라는 단순하고 깔끔한 테마를 선택하여 진행을 하였습니다. `fork`를 떠서 본인의 repository에 옮긴뒤 약간의 커스터마이징을 가미하도록 합시다.

#### _config.yml

Jekyll내의 모든 설정은 `_config.yml`을 통해서 관리합니다. 선택한 테마에서 데모를 위해 설정되어 있는 내용을 이 블로그를 위해 수정을 하도록 합시다.

<figure class="highlight">

    <span class="s">name</span><span class="pi">:</span> <span class="s">배움과 경험을 정리하는 삶</span>
    <span class="s">author</span><span class="pi">:</span>
      <span class="s">name</span><span class="pi">:</span> <span class="s">Hongsik Alex Lee</span>
      <span class="s">email</span><span class="pi">:</span> <span class="s">labyrins@gmail.com</span>
    <span class="s">url</span><span class="pi">:</span> <span class="s">http://labyrins.github.io</span>
    <span class="s">baseurl</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>

    <span class="c1"># Footer에 넣을 소셜아이콘들을 위해 설정</span>
    <span class="s">facebook</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://www.facebook.com/hongsik.lee"</span>
    <span class="s">github</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://github.com/Labyrins"</span>`</pre>

    </figure>

    #### CSS/Font 추가

    영문으로 쓴다면 테마에서 지정한 font로도 큰 문제가 없지만 한글로 작성할 블로그이니 font를 추가하고, 소셜 아이콘으로 쓸 이미지가 담긴 font-awesome을 추가한뒤 `style.css`를 수정하여 적용합니다.

    <figure class="highlight">

    <pre>`<span class="c">&lt;!--header.html--&gt;</span>
    <span class="nt">&lt;link</span> <span class="na">rel=</span><span class="s">"stylesheet"</span> <span class="na">href=</span><span class="s">"{{ site.baseurl }}/css/font-awesome.css"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;link</span> <span class="na">rel=</span><span class="s">"stylesheet"</span> <span class="na">href=</span><span class="s">"https://fonts.googleapis.com/earlyaccess/jejugothic.css"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;link</span> <span class="na">rel=</span><span class="s">"stylesheet"</span> <span class="na">href=</span><span class="s">"https://fonts.googleapis.com/earlyaccess/nanumgothic.css"</span><span class="nt">&gt;</span>`</pre>

    </figure>

    <figure class="highlight">

    <pre>`<span class="c">/* style.css */</span>
    <span class="c">/* 본문 텍스트로 쓰일 font는 클래스 수정 */</span>
    <span class="nt">p</span><span class="o">,</span> <span class="nt">form</span> <span class="p">{</span>
      <span class="nl">font-family</span><span class="p">:</span> <span class="s2">'Nanum Gothic'</span><span class="p">,</span> <span class="nb">serif</span><span class="p">;</span>
      <span class="nl">font-size</span><span class="p">:</span><span class="m">20px</span><span class="p">;</span>
      <span class="err">..</span>
    <span class="p">}</span>

    <span class="c">/* footer용 font로 쓰일 클래스는 신규추가 */</span>
    <span class="nc">.footer-text</span> <span class="p">{</span>
      <span class="nl">font-family</span><span class="p">:</span> <span class="s2">'Jeju Gothic'</span><span class="p">,</span> <span class="nb">serif</span><span class="p">;</span>
      <span class="nl">font-size</span><span class="p">:</span> <span class="m">16px</span><span class="p">;</span>
    <span class="p">}</span>`</pre>

    </figure>

    #### Footer

    링크를 위한 소셜아이콘의 추가와 간단한 문구가 담긴 Footer를 위해 `footer.html`를 아래와 같이 수정합니다.

    <figure class="highlight">

    <pre>`<span class="nt">&lt;footer&gt;</span>
        <span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"inner"</span><span class="nt">&gt;</span>
            <span class="nt">&lt;p</span> <span class="na">class=</span><span class="s">"footer-text"</span><span class="nt">&gt;</span>2017, Hongsik Alex Lee<span class="nt">&lt;br&gt;</span>
                <span class="nt">&lt;a</span> <span class="na">href=</span><span class="s">"{{ site.facebook }}"</span><span class="nt">&gt;</span>
                    <span class="nt">&lt;span</span> <span class="na">class=</span><span class="s">"fa-stack fa-sm"</span><span class="nt">&gt;</span>
                        <span class="nt">&lt;i</span> <span class="na">class=</span><span class="s">"fa fa-circle fa-stack-2x"</span><span class="nt">&gt;&lt;/i&gt;</span>
                        <span class="nt">&lt;i</span> <span class="na">class=</span><span class="s">"fa fa-facebook fa-stack-1x fa-inverse"</span><span class="nt">&gt;&lt;/i&gt;</span>
                    <span class="nt">&lt;/span&gt;</span>
                <span class="nt">&lt;/a&gt;</span>
                <span class="nt">&lt;a</span> <span class="na">href=</span><span class="s">"{{ site.github }}"</span><span class="nt">&gt;</span>
                    <span class="nt">&lt;span</span> <span class="na">class=</span><span class="s">"fa-stack fa-sm"</span><span class="nt">&gt;</span>
                        <span class="nt">&lt;i</span> <span class="na">class=</span><span class="s">"fa fa-circle fa-stack-2x"</span><span class="nt">&gt;&lt;/i&gt;</span>
                        <span class="nt">&lt;i</span> <span class="na">class=</span><span class="s">"fa fa-github fa-stack-1x fa-inverse"</span><span class="nt">&gt;&lt;/i&gt;</span>
                    <span class="nt">&lt;/span&gt;</span>
                <span class="nt">&lt;/a&gt;</span>
            <span class="nt">&lt;/p&gt;</span>
        <span class="nt">&lt;/div&gt;</span>
    <span class="nt">&lt;/footer&gt;</span>`</pre>

    </figure>

    #### jekyll의 빌드와 구동 그리고 draft모드

    포스트의 작성은 `_posts` 디렉토리에 `YYYY-MM-DD-Title.md`형식의 이름을 가진 markdown 파일을 작성함으로써 이루어집니다. 해당 파일이 `_posts`로 들어오면 Jekyll에 의해 정적인 웹페이지로 변환되며, 이는 `_site`에 날짜별 디렉토리로 작성되어 있음을 확인할 수 있습니다. 우선, 우리가 선택한 테마에는 템플릿용으로 `_posts`에 md파일이 있으니 삭제하고 빌드를 해줍니다.

    <div class="highlighter-rouge"><pre class="highlight">`jekyll build
    `</pre>
    </div>

    빌드를 진행하면 현재 `_posts`디렉토리에 있는 md파일을 기준으로 `_site`내의 정적파일이 생성됩니다. 이제 Jekyll을 구동하여 작성한 페이지를 확인하도록 합시다. 기동이 완료되면 `localhost:4000`을 통해 확인가능합니다.

    <div class="highlighter-rouge"><pre class="highlight">`jekyll serve
    `</pre>
    </div>

    한번에 글을 작성하기 어려운 분들을 위한 Draft모드도 있습니다. 프로젝트 루트에 `_drafts`를 생성한뒤 임의의 md파일을 넣고 draft모드로 서버를 구동하면 `_draft`내의 md파일이 현재의 날짜로 세팅이 되어 페이지를 통해 확인이 가능할 수 있습니다. Draft모드로의 서버구동은 아래와 같습니다.

    <div class="highlighter-rouge"><pre class="highlight">`jekyll serve --drafts
    `</pre>
    </div>

    * * *

    이로써 정적인 포스트를 작성하기 위한 모든 작업을 마쳤습니다. Jekyll에서의 포스트 작성은 기본적으로 Markdown의 문법을 사용하지만 Liquid를 사용한 확장표현이 가능합니다. 아래에는 Markdown의 기본문법을 벗어난 좀더 다양한 방법을 통한 사용예제를 소개합니다.

    #### code snippet

    아래와 같이 liquid에서 제공하는 `hilight`과 `endhilight`으로 코드를 감싸 표기할 수 있습니다.

    <figure class="highlight">

    <pre>`<span class="nt">nav</span> <span class="nt">a</span><span class="nd">:hover</span> <span class="p">{</span>
      <span class="nl">color</span><span class="p">:</span> <span class="n">rgba</span><span class="p">(</span><span class="m">0</span><span class="p">,</span><span class="m">0</span><span class="p">,</span><span class="m">0</span><span class="p">,</span><span class="m">.72</span><span class="p">);</span>
    <span class="p">}</span>
    <span class="nt">nav</span> <span class="nt">a</span><span class="nc">.current</span> <span class="p">{</span>
      <span class="nl">color</span><span class="p">:</span> <span class="n">rgba</span><span class="p">(</span><span class="m">0</span><span class="p">,</span> <span class="m">0</span><span class="p">,</span> <span class="m">0</span><span class="p">,</span> <span class="m">.72</span><span class="p">)</span>
    <span class="p">}</span>
    <span class="nc">.subtitle</span> <span class="p">{</span>
      <span class="nl">margin</span><span class="p">:</span> <span class="m">30px</span> <span class="m">0</span><span class="p">;</span>
    <span class="p">}</span>`</pre>

    </figure>

    #### 타이틀 이미지 설정

    페이지 상단에 멋진 이미지를 삽입하여 페이지의 품격을 높이기 위해서는 프로젝트 루트의 `/img/covers/`디렉토리 안에 원하는 이미지를 넣은뒤 Markdown문서 상단 페이지 속성에 `cover-image : 이미지 파일명`을 선언합니다. 본 페이지 상단과 같이, 삽입된 이미지는 자동으로 하단으로 갈수록 이미지가 fadeout 됩니다.

    #### 이미지 삽입

    모든 이미지는 `img`디렉토리에서 관리합니다. 여타의 Markdown문법과 같이 사용하고, URL선언을 현재 프로젝트 내의 파일로 지정하면 손쉽게 이미지를 삽입할 수 있습니다. `site.baseurl`은 `_config.yml`에 선언되어 있음을 확인합시다. 출처를 명시할 때는 `&lt;small&gt;&lt;/small&gt;`를 사용합니다.

    <div class="highlighter-rouge"><pre class="highlight">`![Forest]({{ site.baseurl }}/img/howtoyoutubeimport.png)
    `</pre>
    </div>

    #### 인용문 삽입

    사용하려는 인용문을 `&gt;`로 감싸 표기합니다. 이미지의 삽입과 마찬가지고 인용절 뒤에 `&lt;small&gt;&lt;/small&gt;`을 통해 출처 등을 남길 수 있습니다.

    > It’ll be nipper heaps trent from punchy oldies. Trent from punchy no dramas when flat out like a tucker-bag. He hasn’t got a piker flamin frog in a sock.
>     <small>— [Bogan Ipsum](http://boganipsum.com/)</small>

    #### Youtube 영상 삽입

    Youtube영상을 삽입하기 위한 [FitVids.js](http://fitvidsjs.com/)가 추가되어 있습니다. `iframe`을 사용하여 삽입을 합니다. 아래와 같이 유튜브 영상페이지로부터 삽입할 소스코드는 손쉽게 구할 수 있습니다.

    ![Forest](http://labyrins.github.io/img/howtoyoutubeimport.png)

    <iframe width="560" height="315" src="https://www.youtube.com/embed/i1n_1jrUEjU" frameborder="0" allowfullscreen=""></iframe>

    #### Tables 삽입

    Table의 삽입은 [Github-Flavored-Markdown](https://help.github.com/articles/github-flavored-markdown/#tables)에 기반한 Markdown문법을 사용합니다. 자세한 문법은 위의 링크를 참고바랍니다.

    <div class="highlighter-rouge"><pre class="highlight">`| Left-Aligned  | Center Aligned  | Right Aligned |
    | :------------ |:---------------:| -----:|
    | col 3 is      | some wordy text | $1600 |
    | col 2 is      | centered        |   $12 |
    | zebra stripes | are neat        |    $1 |

</div>

<table>
  <thead>
    <tr>
      <th style="text-align: left">Left-Aligned</th>
      <th style="text-align: center">Center Aligned</th>
      <th style="text-align: right">Right Aligned</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">col 3 is</td>
      <td style="text-align: center">some wordy text</td>
      <td style="text-align: right">$1600</td>
    </tr>
    <tr>
      <td style="text-align: left">col 2 is</td>
      <td style="text-align: center">centered</td>
      <td style="text-align: right">$12</td>
    </tr>
    <tr>
      <td style="text-align: left">zebra stripes</td>
      <td style="text-align: center">are neat</td>
      <td style="text-align: right">$1</td>
    </tr>
  </tbody>
</table>

이로써 페이지의 Jekyll - Github Page를 통한 블로그 준비와 포스트 작성에 관한 모든 것이 정리 되었습니다. Tag기능과 SEO를 위한 설정은 추후에 세팅을 하도록 하고 이제부터는 지식을 쌓기만 하면 됩니다. 강철의 근성이 공부를 하는 여러분들에게 가호를 내리기 바라며..