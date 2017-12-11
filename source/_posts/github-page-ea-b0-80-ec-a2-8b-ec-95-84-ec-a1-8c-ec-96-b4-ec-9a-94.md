---
title: GitHub page가 좋아졌어요.
tags:
  - github-page
id: 417
categories:
  - Small talk
date: 2016-12-11 19:26:21
---

말하기도 입아픈 소셜 코딩 사이트, GitHub에서 새로운 기능을 발표했습니다. 프로젝트의 웹페이지나, 개발들의 블로그 등으로 이용할 수 있었던 GitHub Page의 기능이 좋아졌습니다.

1.  마크다운을 그대로 보여줍니다. 기존에는 gh-pages라는 별도의 브랜치를 만들어야했는데, 이제는 그럴필요 없이 설정에서 브랜치를 지정하면 됩니다. 또 마스터 브랜치 외에 doc 폴더가 있으면 그걸 바로 지정할 수도 있어요.
2.  파일이름도 index 가 아니어도 되요. index.html, index.md 외에도 README.md 파일만 있어도, 페이지를 보여줍니다. ( 확장자가가 md가 아니면 렌더링 안해줍니다. )

자세한 내용은 아래 원문의 번역을 참고하세요.

( 원문 : [Publishing with GitHub Pages, now as easy as 1, 2, 3](https://github.com/blog/2289-publishing-with-github-pages-now-as-easy-as-1-2-3) )

깃헙 페이지를 발행하기가 이제 쉬워졌습니다. 1, 2, 3 단계만 따라하세요.

웹사이트나 소프트웨어문서를 깃헙 페이지를 통해서 발행하기 위해서 필요한 단계가 더 적어졌습니다. 정확히 세 단계입니다.

<div class="blog-post-body markdown-body">

1.  [리파지토리를 만드세요.](https://github.com/new) (아니면 사용하시는 리파지토리로 이동하세요.)
2.  다른 파일을 커밋하드시, 웹페이지에서 마크다운 파일을 커밋하세요.
3.  리파지토리 설정에서 깃헙 페이지 기능을 켜세요.
그리고 이게 다예요. - 이제 웹사이트가 생겼습니다. 깃헙페이지에 익숙하다면, 어떻게 동작하는지 궁금하실건데, 웹페이지를 발행하는 단계를 간단하게 만들었고, 깃헙 어디서건 만들어진 마크다운 문서를 가져오도록 했습니다.

</div>

1.  이제 모든 마크다운 문서는 깃헙페이지에서 보여지게 됩니다. 각각의 파일에 YAML로 된 첫 단계만 추가하면 됩니다. ( 메타 데이터는 ---s 로 분리해서 처음에 작성하면 됩니다. )
2.  index.md( 또는 index.html)이 없다면 README파일을 인덱스로 사용합니다. 깃헙 리파지토리에서 파일을 브라우징할때 때랑은 다릅니다.
3.  사이트 설정에서 테마를 설정하지 않았다면(또는 설정파일을 가지고 있지않다면), 깃헙에서 마크다운이 보이는 대로, 최소한의, 기본 테마로 설정됩니다.
4.  레이아웃을 지정하지 않으면, 내용에 기초해서 정합니다. 예를 들어, 페이지 레이아웃이 없다면, 페이지는 자동으로 페이지 레이아웃으로 보이거나, 기본 레이아웃으로 보여집니다.
5.  페이지의 제목이 명시적으로 지정되지 않았고 H1, H2 또는 H3로 시작한다면, 페이지의 제목으로 사용합니다. 제목은 브라우져 탭의 제목으로 보입니다.

이번 개선으로 처음으로(또는 여러번해왔던 ) 웹사이트 발행이 몇번의 클릭만으로, 또는 소프트웨어 프로젝트의 문서화가 마크다운 파일을 만들어두거나 docs라는 문서 폴더를 만들어주는 것 만으로 이루어지게 됬습니다. 물론 계속해서 페이지의 보이는 모습을 수정할 수 있습니다. ( 레이아웃이나 스타일의 변경 같은.)

이번 변경사항이 이전에 만들어진 사이트에 영향을 미치지 않기를 바라지만, 일부 jekyll 사용자에게는 두가지 잠재적인 문제가 발견되었습니다.

1.  사이트가 모든 페이지에서 반복한다면( 예를들어 for page in site.pages ), 몇가지 추가된 페이지를 목록에서 발견할 수 있습니다. ( README 같은 ) 설정파일에서 명시적으로 표시하지 않도록 설정할 수 있습니다.
2.  페이지 레이아웃이나 제목을 지정하지 않아야할는 경우( 예를들어 스타일이 적용되지 않는 내용을 보여줘야 하는 경우)에는, 그러한 값을 null 로 명시적으로 지정해야 합니다.

그리고 다른 이유때문에 이러한 기능을 사용하지 않기를 원한다면, 최상위 폴더에 .nojekyll 파일을 추가하면 됩니다.

깃헙 페이지의 빌드프로세스를 가능한한 수정가능하고 투명하게 하기 위해서 위의 모든 기능은 오픈소스 jekyll 플러그인을 사용해서 구현되었으며, 그것은 다음과 같습니다. [Jekyll Optional Front Matter](https://github.com/benbalter/jekyll-optional-front-matter), [Jekyll README Index](https://github.com/benbalter/jekyll-readme-index), [Jekyll Default Layout](https://github.com/benbalter/jekyll-default-layout), and [Jekyll Titles from Headings](https://github.com/benbalter/jekyll-titles-from-headings).

<div class="blog-post-body markdown-body">

다시한번 이러한 변화가 기존에 만들어진 대부분의 사이트를 만드는데에 영향을 주지 않지만(비록 이 기능을 사용할 수 있지만), 혹시 문제가 있다면 [연락주시기 바랍니다](https://github.com/contact?form%5Bsubject%5D=Simplified%20Pages%20publishing).

</div>

3단계로 발행하는것을 즐기세요!

<div class="blog-post-body markdown-body">

&nbsp;

</div>