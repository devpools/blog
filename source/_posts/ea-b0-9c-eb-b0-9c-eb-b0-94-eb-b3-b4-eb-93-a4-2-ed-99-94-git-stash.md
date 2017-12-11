---
title: 개발바보들 2화 – git “Stash”
id: 749
categories:
  - Cartoon
date: 2017-02-27 19:50:44
tags:
---

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/01-e1488948109290.png)

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/1-640x1024.png)

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082859.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082858-2.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082858-1.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082858.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082856.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082854.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082852.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082850.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082848.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082846.png) ![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/20170313_082844.png) 세상을 살아갈 때, 내가 계획한 대로 되는 일이 거의 없드시 개발도 마찮가지로, 다양한 상황을 겪게 됩니다. 이런 예상치 못한 일을 겪게 될 때, 유연하게 대처하는 방법 중 하나가  stash 입니다.  stash는 Git에서 사용하는 임시저장 명령입니다.

* * *

다음과 같은 상황을 한번 생각해보세요.

1.  다음 버전의 신규 기능을 개발하고 있는데, 며칠 전 배포한 버전의 기능에서 문제가 생겨서, 긴급히 수정해야하는 상황.
2.  코딩을 한참 했는데, 커밋을 하려고 보니 master 브랜치<del>( 헛!! )</del>인 상황.

&nbsp;

1번의 경우라면 임시로 커밋하고 릴리즈 브랜치로 이동해서 버그 패치를 만들 수 있을텐데, 2번의 경우는 그마저도 좀 어렵죠. 네. 이미 예상하셨드시 stash를 사용하면 간단합니다. 그러면 stash에 대해서 좀 더 자세히 설명해 볼께요.

stash의 임시 저장은 저장하고 불러오고가 기본입니다. `git stash` 와 `git stash pop`만 아시면 되요. 임시 저장하고, 임시 저장된 내용을 가져오고 임시저장된 내용을 거죠. 쉽죠? 그런데 이렇게 하면 임시저장소를 하나만 사용하는 거예요. ( 그렇게 까지 사용하시지는 않겠지만) stash의 임시 저장공간 갯수는 제한이 없습니다.  그래서 pop 말고도 여러가지 명령이 있습니다.

*   **저장하기 (SAVE)** "_git stash save [메시지]_"
"_git stash_" 명령은 save 옵션이 생략된 축약형입니다. 그대로만 사용하셔도 되지만, stash에 메시지를 지정할때는 save를 명시 해줘야 합니다.
*   **목록보기(LIST)** "_git stash list"
_stash에 저장된 목록을 봅니다. stash@{숫자}의 형식으로 보여지게 되는데, 가장 최근에 stash된것이 0번이고, 이후로 저장하게 되면 순서가 밀리게 됩니다. 그래서 항상 0번이 최신이고 1,2 .. 순서로 밀려서 저장됩니다.
*   **불러오기(APPLY)** "_git stash apply stash@{숫자}_"
지정된 stash를 불러옵니다. 이때, stash를 지정하지 않으면 가장 최신의( stash@{0}) 을 불러옵니다. stash될 때 인덱스에 추가된 상태로 적용하고 싶다면 --index 옵션을 주시면 됩니다.
*   **삭제하기(DROP)** "_git stash drop [stash@{숫자}]_"
지정된 stash를 삭제합니다. 마찬가지로 stash를 지정하지 않으면 최신의 stash를 삭제합니다.
*   **불러오고 삭제하기(POP)** "_git stash pop [stash@{숫자}]_"
apply와 drop을 한번에 수행합니다. 지정된 stash를 불러오고 삭제합니다. 마찬가지로 stash를 지정하지 않으면 최신의 stash를 삭제합니다.
*   **내용 보기(SHOW)** "_git stash show[stash@{숫자}]_"
stash 된 내용을 확인합니다. 마찬가지로 stash를 지정하지 않으면 최신의 stash를 보여줍니다.
*   **브랜치로 만들기(BRANCH)** "_git stash branch &lt;새로만들브랜치이름&gt; [stash@{숫자}]_"
stash 된 내용으로 새로운 브랜치를 만듭니다. 이때, pop과 마찬가지로 stash 된 내용은 삭제됩니다.

&nbsp;

stash는 여러개를 저장할 수 있고, 그것들을 골라서 적용할 수 있다, 그리고 stash된 걸로 브랜치를 만들 수 있다라고 알아두세요. ( 어차피 커맨드로 안하시잖아요? )

그럼 다시 위의 상황을 다시 보시죠.

첫번째 경우에서는 `git stash`했다가 패치를 만들고 나서 다시 내 브랜치로 돌아와서 `git stash pop`하면 되고, 두번째 경우에서는 `git stash branch feature-mymy`처럼 브랜치를 생성하면 됩니다. 알고보니 참 쉽죠? <del>( 이제 임시커밋, 쩜 찍고 커밋 같은 걸로 혼나지 맙시다 ㅠㅠ )</del>

&nbsp;

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/2-1-640x1024.png)