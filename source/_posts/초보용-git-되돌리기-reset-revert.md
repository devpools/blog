---
title: '[초보용] Git 되돌리기( Reset, Revert )'
tags:
  - git
  - reset
  - revert
id: 590
categories:
  - Dev-Tips
date: 2017-02-05 16:59:57
---

개발바보들 1화 [git "back to the future"](http://devpools.kr/2017/01/31/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-1%ED%99%94-git-back-to-the-future/)에서 설명한 Reset / Revert에 대한 글입니다.

&nbsp;

Git을 익히면서 헷갈렸던 것들 중의 하나가 이력을 되돌리기 입니다. Git에서 이력을 되돌리는 방법은 여러가지가 있지만, 그 중에 대표적인게 Reset과 Revert 입니다. 단어 의미만 보고는 둘 사이의 차이를 알기 쉽지 않은데, 풀어서 설명해보면 Reset은 시계를 다시 맞추드시 이력을 그 당시로 되돌리는 것이고, Revert는 이전 이력은 그대로 두고, 그 되돌릴 커밋의 코드만 원복시킵니다. 이 개념을 이리온님께서 만화로 설명해 주신게 있는데 참고하시면 이해하는데, 더 도움이 됩니다.

( [개발바보들 1화 — git “Back to the Future ”](http://devpools.kr/2017/01/31/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-1%ED%99%94-git-back-to-the-future/))

&nbsp;

<span style="font-size: 18pt;">1\. Reset</span>

앞에서 설명한대로 Reset은 시계를 다시 맞추는 것입니다. 돌아 가려는 커밋으로 리파지토리는 재설정되고, 해당 커밋 이후의 이력은 사라집니다. 예를 한번 들어볼까요? ( 일반적인 개발 이력과는 차이가 있지만, 이해가 쉽게 하기 위해 영화 “유주얼 서스펙트"를 이용했고 이에 대한 스포일러를 포함합니다. 하지만, 이미 보셨거나 들어보셨을 것이라 생각합니다. )

![](/images/2017/02/1-XKvSxRueC2HYlGj1O72woA.png "그림1\. 기대했으나 스포일러 때문에 실망했던 이력")

<span style="font-size: 10pt;">그림1\. 기대했으나 스포일러 때문에 실망했던 이력</span>

[그림1]을 보시면 기대했던 영화를 예매하였으나 스포일러 때문에 실망했던 이력을 볼 수 있습니다. 그래서 스포일러를 보기 전으로 이력을 되돌리기로 합니다. 마치 내가 기억하고 있는 내용을 변경하는 거죠. 커밋 a3bbb3c 이후의 기억은 지우고 싶습니다. Reset은 다음과 같이 사용합니다.

&nbsp;

<pre id="5407" class="graf graf--pre graf-after--p">$ git reset &lt;옵션&gt; &lt;돌아가고싶은 커밋&gt;</pre>

&nbsp;

여기에 옵션이 몇가지 있는데 자주 쓰는 것 hard, mixed, soft 세가지가 있습니다. 영화를 예매하고 검색한 이력인 a3bbb3c 이후에 발생했던 ( 표를 예매하고, 팝콘과 사이다를 구매 같은)변화에 대해서 어떻게 할지에 대한 것입니다.

&nbsp;

(1) hard

돌아가려는 이력이후의 모든 내용을 지워 버립니다. 이렇게 하면 표를 예매하고, 팝콘과 사이다를 구매했던 모든 것들이 지워지고 모든것이 초기화 됩니다.

&nbsp;

<pre id="8053" class="graf graf--pre graf-after--p">$ git reset --hard  a3bbb3c</pre>

&nbsp;

![](/images/2017/02/2.png)

<span style="font-size: 10pt;">그림2\. hard 옵션으로 reset한 후의 이력</span>

&nbsp;

(2) soft

돌아가려 했던 이력으로 되돌아 갔지만, 이후의 내용이 지워지지 않고, 해당 내용의 인덱스(또는 스테이지)도 그대로 있습니다. 바로 다시 커밋할 수 있는 상태로 남아있는 것입니다. 기억은 되돌려졌지만, 표와 팝콘과 사이다는 손에 들려있는 상태입니다.

&nbsp;

<pre id="1778" class="graf graf--pre graf-after--p">$ git reset --sorf a2bbb3c</pre>

&nbsp;

![](/images/2017/02/3.png)

<span style="font-size: 10pt;">그림3\. soft옵션으로 reset한 후의 이력</span>

![](/images/2017/02/4.png)<span style="font-size: 10pt;">그림4\. soft옵션으로 reset한 후의 파일 상태</span>

<figure id="fc29" class="graf graf--figure graf-after--figure">

<figcaption class="imageCaption"></figcaption>

</figure>
(3) mixed ( 옵션을 적지 않으면 mixed로 동작합니다. )

역시 이력은 되돌려집니다. 이후에 변경된 내용에 대해서는 남아있지만, 인덱스는 초기화 됩니다. 커밋을 하려면 다시 변경된 내용은 추가해야 하는 상태입니다. 기억도 되돌려 졌고, 표와 팝콘 그리고 사이다는 사야겠다는 마음만 남아있다고 할 수 있습니다.

&nbsp;

<pre id="e6c8" class="graf graf--pre graf-after--p">$ git reset --mixed a2bbb3c</pre>

&nbsp;

![](/images/2017/02/5.png)

<span style="font-size: 10pt;">그림5\. mixed 옵션으로 reset한 후의 이력</span>

![](/images/2017/02/6.png)<span style="font-size: 10pt;">그림6\. mixed 옵션으로 reset한 후의 파일 상태</span>

&nbsp;

또 되돌아가는 커밋을 커밋 해쉬를 통해서 직접 지정할 수도 있고 현재부터 몇개의 커밋을 되돌릴 수도 있습니다 [그림1]에서 처럼 15413dc 부터 a3bbb3c로 돌아가려면

&nbsp;

<pre id="bebd" class="graf graf--pre graf-after--p">$ git reset HEAD~6</pre>

&nbsp;

위와 같이 현재부터 6개 이전 이력으로 돌아가라라고 상대적으로 지정할 수도 있습니다.

&nbsp;

<span style="font-size: 18pt;">2\. Revert</span>

Revert는 상태를 되돌린다고 볼 수 있습니다. 스포를 당한 커밋을 revert하고 현재 작성중인 코드만 본다면 reset과 동일한 (hard 옵션 준거만 빼고) 결과를 가집니다. 하지만 이력은 같지 않습니다. 먼저 결과를 먼저 보고 이어가겠습니다. (reset과 동일하게 스포일러를 당한 것을 되돌립니다)

![](/images/2017/02/7.png)

<span style="font-size: 10pt;">그림7\. 스포일러 당한 커밋을 되돌림</span>

이전 이력은 그대로 있고, 스포일러를 당했던 커밋만을 되돌렸습니다. 마치 스포일러 당한것에 대한 것을 기억하고 있지만, 그 내용은 알지 못하는 것처럼 말이죠. ( 이 내용은 앞에서 언급했던 Devpools의 설명에 나온 모나리자 눈썹의 내용이 더 이해가 쉬울것 같습니다. )

revert 를 하는 방법과 스포일러 댓글의 커밋을 되돌리는 것은

&nbsp;

<pre id="7a19" class="graf graf--pre graf-after--p"># git revert &lt;되돌릴 커밋&gt; 
git revert 2664ce8</pre>

&nbsp;

이고 되돌릴 커밋이 여러개라면 범위를 주어서 여러개를 선택할 수도 있습니다. [그림1]에서 예를 들면 댓글을 읽은 것부터 영화관을 나설때까지 모두 되돌리려면 아래 코드처럼 범위를 주시면 됩니다.

&nbsp;

<pre id="2860" class="graf graf--pre graf-after--p graf--last">git revert 2664ce8..15413dc</pre>

&nbsp;

<article class=" u-sizeViewHeightMin100 u-overflowHidden postArticle postArticle--full is-withAccentColors" lang="ko" data-scroll="native">
<div class="postArticle-content js-postField js-notesSource js-trackedPost" data-post-id="d572b4cb0bd5" data-source="post_page" data-collection-id="32e8039ca279" data-tracking-context="postPage" data-scroll="native"><section class="section section--body section--last">
<div class="section-content">
<div class="section-inner sectionLayout--insetColumn"></div>
<div class="section-inner sectionLayout--insetColumn">

&nbsp;

<span style="font-size: 18pt;">3\. 언제 reset을 하고 언제 revert를 해야하나?</span>

단순하게 생각하면 reset을 하는 것이 revert를 하는 것보다 이력을 더 단순하게 만들어주기 때문에 revert의 장점이 많지 않아 보입니다. 하지만 이력 중간에 로그 출력하도록 한 커밋이 있고 그 커밋만을 취소하려고 한다면 reset을 사용하여 이후의 이력을 모두 제거하는 것은 이후 이력을 모두 날려버리는 결과를 나을 것입니다. 이런 때 revert를 사용하여 해당 커밋의 내용만 되돌릴 수 있습니다. 또한 이미 원격 리파지토리에 push 를 한 상태라면 reset을 사용하면 reset 하기 이전으로 되돌리기 전까지는 push 할 수 없게됩니다. (물론 force라는 무시무시한 옵션이 있기는 합니다. ) 그래서 이미 push 한 코드라면 미련을 버리고 revert를 하셔야 합니다.

&nbsp;

</div>
</div>
</section></div>
<footer class="u-paddingTop10">
<div class="container u-maxWidth740"></div>
</footer></article>