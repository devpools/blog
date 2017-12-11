---
title: ES2015 리팩토링 - 1. 몸풀기
id: 709
categories:
  - JavaScript
  - ReactJs
  - Refactoring
date: 2017-03-06 17:19:24
tags:
---

ES2015는 프론트엔드 개발자들에게 많은 화두를 던졌고 아주 진지하게 자바스크립트(JavaScript)가 세상을 집어삼킬 거라는 포부를 던지고 있다. 가지고 있던 언어적인 난해함을 벗어던지고 더 많은 개발자들이 좀 더 손쉽게 Java와 C 처럼 개발할 수 있도록 많은 개념들을 제시했다. 여타 다른 객체언어처럼 클래스(class)를 도입하고 화살(arrow)함수를 도입한게 그 좋은 예라고 볼 수 있을 것이다.

&nbsp;

지금부터 시작하는 시리즈는  ES2015가 가고자 하는 방향에 맞춰 예전에 만들었던 많은 프로젝트들을 리팩토링 해 볼 것이다. 순서는 아마도 다음과 같이 진행이 될 것이다. (중간에 순서와 목차는 바뀔 수 있다)

1.  몸풀기
2.  빠레꽁(var let const)
3.  함수와 모듈화가 JavaScript에 당췌 왜 필요할까
4.  Promise 혹은 Async. 그래도 콜백지옥은 피할 수 없어
5.  JavaScript로 바라보는 함수형 프로그래밍의 미래
6.  이터레이터를 어떻게 쓸까
7.  타입스크립트 도입은 바람직할까

&nbsp;

# 몸풀기

시작하기 전에ES2015를 지금 당장 시작할 수 있을까 하는 생각을 하고 있는 사람이 있을텐데 지금은 어느덧 2017년. 이미 스펙은 오래전부터 안정화 되어 있다.

![](http://devpools.kr/wordpress/wp-content/uploads/2017/03/20170306_083618.jpg)

링크 : [https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/)

## 트랜스파일러를 선택하자. <del>꼬오오오오옥이요</del>.

위의 사이트 통계에 따르면 사파리는 100%, Edge14는 93%, chrome은 97%, FireFox는 94% 정도의 compatibility를 보장하고 있으니 최신 브라우저를 사용한다고 하면 대부분은 그냥 쓸 수 있다고 보면 될 것으로 보인다. 그럼에도 불구하고 ES2015를 쓰려고 하면 필자가 세가지 정도 이유에서 아직은 babel 같은 transpiler를 써야하는데 첫번째는 가장 중요한 것으로 보이는 모듈화에 대한 스펙이 ES2015에는 올라왔지만 번들링이슈를 포함한 개발환경과 브라우저에 동작하는 것이 다른 부분에 대한 어려움 때문이고 두번째는 아직 Internet Explorer를 많이 쓰고 있는 한국 환경이 걸림돌이고 마지막으로는 성능이슈이다.

첫번째 이야기 모듈에 대한 이야기는 좀 복잡한데 간단하게 설명하자면 ES2015 개발 환경이라면 아래 처럼 코드는 사용될 것이다

import somemodule from 'ModuleFile'

이런 식이라면 아래 그림처럼 상대 경로를 통해 해당 함수에 대한 로딩이 이루어져하는데 현재 번들링하는 관점에서는 비효율적으로 동작할 것이다.

![](http://devpools.kr/wordpress/wp-content/uploads/2017/02/importing-1024x485.png)

Button 액션을 담당하는 파일을 로딩해서 그 함수를 사용하게 되는 것이지만 일반적으로 작성하는  HTML을 생각해 보면 보통 JavaScript를 저렇게 로딩하고 있지 않고 아래처럼 script 태그를 이용해서 로딩을 시도하게 된다.

&lt;script src="someScript.js"/&gt;

이렇게 소스코드를 만들 때에는 보통 번들링이라는 형태로 여러파일들을 하나로 묶거나 (unify)  줄이는 ( uglify)를 하게 되는데 이런 과정을 거치는 이유는 js 파일을 다운로드 받는 속도의 문제와 보안문제 때문이다.

또 한꺼번에  js 파일들을 모두 로딩하는 것이 비효율적이라는 생각을 가지고 AMD 스펙같은데에서는 다이나믹 로딩을 사용하고 있다. 마침 dynamic import 에 대한 spec 또한 tc39의 3단계까지 올라가 있다

https://github.com/tc39/proposal-dynamic-import

아무래도 이게 정해지고 나면 import 에 대한 브라우저들의 지원이 확실해 질 듯하고 아마 모듈 로딩에 대해서도 코드 표준들이 바뀔 수 있을 것으로 보인다.

두번째 이유는 <del>더이상 자세한 설명은 생략한다</del>

세번째는 ES2015의 Iterator라던지 변수 scope등은 이전까지 JavaScript에 대해 잘 알지 못하던 사람들이 실수하던 코드를 다른 언어처럼 짤 수 있게 한 부분이 많아서 왠만한 개발자들이 짜던 것보다 코드 품질이 좋게 변환을 해 준다.

[contentcards url="http://sculove.github.io/blog/2016/07/26/Babel-%EB%84%88-%EB%AF%BF%EC%9D%84%EC%88%98-%EC%9E%88%EB%8B%88/" target="_blank"]

관련 블로그를 링크를 거는데 중요한 문단은 다음과 같다.

> #### "한마디로 정리한다면… 안타깝지만, Babel 은 저 보다 코드를 더 잘짰습니다. ㅠㅠ"

&nbsp;

## 그럼 어떤 트랜스파일러를 사용할 것인가

어떤 프레임워크를 사용하는가에 따라서 트랜스파일러도 보통은 선택되는 상황이지만 크게 babel과 traceur 로 양분이 되는데 작년 기준이기는 하지만 거의 babel의 압승으로 끝나는 듯한 모습이다.

https://www.reddit.com/r/javascript/comments/33c9la/which_es6_transpiler/

타입스크립트의 경우는 약간 다른 경우니까 나중에 다뤄보도록 하겠다.

## Linter를 사용할 것

Linting이라고 하는 것은 기본적으로 버그가 날 수 있는 코드를 표시를 해주는 툴로써 이바닥의 요다인 Douglas 옹이 JSLint를 만들고 가장 많이 사용되는 툴은 ESLint  툴이다. 많은 에디터가 ESLint 플러그인을 제공하고 있으므로 꼭 설치하고 시작하는 것이 좋다.

Webstrom 관련된 내용은 기존의 아티클을 읽어보기를 추천한다.

[![](http://devpools.kr/wordpress/wp-content/uploads/2017/03/20170306_070647.jpg)](http://devpools.kr/2017/02/16/javascript-eslint-webstorm/)

&nbsp;

## 스타일 가이드를 활용하자

JavaScript 관련해서 좋은 회사의 스타일 가이드들을 활용하는 것은 매우 추천할 만한 일이다. JavaScript관련해서 가장 핫한 스타일 가이드 중 하나는 airbnb의 스타일 가이드이고 이후 이어지는 글들은 상당히 많이 참조할 것이다.

https://github.com/airbnb/javascript

## 기존 프로젝트를 재활용

아무래도 이런 리팩토링은 legacy 프로젝트가 있어야 한다. 기존에 만들었던 서버리스 아키텍처 관련 프로젝트를 legacy로 리팩토링 해 보도록 하겠다.

https://github.com/ehrudxo/standup