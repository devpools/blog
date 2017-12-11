---
title: REACT 컴포넌트
id: 360
categories:
  - ReactJs
date: 2016-07-03 23:52:43
tags:
---

React 컴포넌트를 작성하는 방법은 크게 두가지가 있습니다.(정확하게는 세가지가 있습니다.)

하나는 React.createClass 메쏘드를 이용하는 방법이고 또 다른 방법은 ES2015의 class extends 를 사용하는 방법입니다.

React.createClass 로 컴포넌트를 작성할때와 ES2015의 class extends를 사용하는 방법은 다음과 같습니다.

https://gist.github.com/ehrudxo/abea06e5da36555cf3fbc878920faca7

이렇게 코드를 들여다 보면 의구심이 듭니다. React.createClass를 통해 Class생성을 하는 것과 ES2015 class extends를 이용하는 것은 어떤 차이가 있을까요?

### 실행환경에서 살펴보기

#### React.createClass

먼저 코드를 짜 보겠습니다.

html 코드와 js 코드는 다음과 같습니다.

https://gist.github.com/ehrudxo/35105441ea195edfbf1bfffea9adb820

&nbsp;

위와 같이 javascript 코드를 짜면 손 쉽게 React.createClass를 이용해서 만든 코드가 실행환경에서 어떻게 보이는지 확인해 볼 수 있습니다.

브라우저의 console 도구를 활용해서 본 클래스는 실제로는 function으로 정의가 되고 다음과 같이 보여집니다.

![React.createClass](http://devpools.kr/wordpress/wp-content/uploads/2016/07/React.createClass-235x300.jpg)

var로 지정한 ReactComponent를 이름으로 가지고 prototype에 여러가지 속성들을 가지는데 한번 눈여겨 볼 필요가 있습니다.

componentDidUpdate, componentWillMount, componentWillReceiveProps, componentWillUnmount, componentWillUpdate 등은 바로 뒤에 다룰 생명주기 함수들과 연관이 있습니다. 하지만 render 말고는 모두 null 로 셋팅이 되어 있는 것도 왜 그럴까 라고 생각해 보면 좋은 학습이 될 거 같습니다.( DeepDive#1)

#### ES2015 class

ES2015 의 코드를 똑같이 짜 보도록 하겠습니다.

코드는 다음과 같습니다

https://gist.github.com/ehrudxo/a54614c19a2d0a69f1aa654a17e2690c

어떤 일이 벌어질까요?

![ES2015.Class](http://devpools.kr/wordpress/wp-content/uploads/2016/07/ES2015.Class-300x149.jpg)

어떤가요? React.createClass 와 비교해 보면 어떤 형태인가요?

함수 이름은 ES2015Component로 바뀌었는데, prototype 부분에 있어야 할 많은 속성, 혹은 함수들이 보이지 않습니다.

왜 이런 일이 벌어지는 걸까요? 이에 대한 답을 하기 전에 아까 두가지라고 하고는 괄호로 세가지라고 힌트를 준 걸 기억하실 겁니다. 다음 내용을 먼저 보시죠.

#### function 만으로 컴포넌트 생성

코드는 다음과 같습니다.

실행을 하면 어떤 일이 벌어질까요?

https://gist.github.com/ehrudxo/93406b42f748426dd1624a24877cb768

![PureFunction01](http://devpools.kr/wordpress/wp-content/uploads/2016/07/PureFunction01-300x71.jpg)

보시는 바와 같이 훌륭하게 페이지에 출력 됩니다.

콘솔 도구를 확인해 보겠습니다.

![PureFunction02](http://devpools.kr/wordpress/wp-content/uploads/2016/07/PureFunction02.jpg)

훨씬 간단한 함수가 있다는 사실을 알게 됩니다.

#### 왜 이렇게 돌아가는 걸까.

[facebook 공식 페이지에 따르면](https://facebook.github.io/react/docs/glossary.html) ReacDOM.render 함수의 첫번째 arguments는 (ReactElement, HTMLElement | SVGElement)를 받을 수 있도록 되어 있습니다.

결국 순수 함수라고는 했지만 이 함수가 하는 일은 HTML Element 를 반환하는 것 뿐입니다.

즉 다음과 같이 표현식을 바꾸는 거랑 똑같은 역할을 한다는 것입니다.

https://gist.github.com/ehrudxo/c2464a81fe005d5e889dbef2802b08a5

이렇게 그려 넣고 보니 어떤가요?

너무 당연한걸 돌아 돌아 설명을 한 거 같습니다. 처음 서두에 React 컴포넌트를 생성하는 방법은 두가지 라고 설명을 드렸습니다만, 반전으로 말씀드리면 Javascript Syntax로 클래스 라고 생각되는 방법을 크게 두가지로 알면 될 것 같습니다.

<pre>조금 더 심화의 내용으로 설명드리면 ES2015를 가더라도 브라우저가 인식하는 java 같은 언어에서 얘기하는 class라는 개념은 실제로는 존재하지 않습니다.
보셔서 아시겠지만 function 이자 prototype 을 이용해서 React가 활용을 하는 셈이지요.
관련 내용에 대해서 더 알고 싶으신 분을 위해서는 Javascript Definite Guide 책을 추천드립니다.
</pre>

#### 그럼 어떤 방식으로 짜는 게 좋을까요?

Airbnb 는 각 개발언어의 style guide를 훌륭하게 내기로 유명한데 [React 용 style 가이드](https://github.com/airbnb/javascript/tree/master/react)도 공개했습니다.

아래와 같은 원칙을 갖고 있네요.

https://gist.github.com/ehrudxo/822b121cd680a23cf6cf6d704d6f505f

생명주기와 관련된 함수가 사용되지 않는 이상(stateless) pure function 으로 사용하는 것이 낫다고 하는데, 여기에 대해서는 장기적인 performance 이슈를 그 예로 들고 있습니다. 이와 관련해서는 저도 찬성입니다.

<pre>**즉, 단순히 HTML을 구조화 하는 state 가 변할 필요학 없는 tag에 대해서는 javascript function 을 사용할 것. 그리고 그 경우가 아니면  ES2015 로 갈 것.
**</pre>

을 원칙으로 하면 될 것 같습니다.

저도 글을 작성할 때에 ES2015기준으로 작성하되, 설명을 위한 특별한 기준이 필요할 때에는 혼용해서 사용할 수 있음을 밝혀둡니다.

#### ES2015로 가면 모든 기능에 문제가 없을까?

일단 ES2015를 이용하려면 브라우저 호환성을 위해 [Babel](https://babeljs.io/) 같은 transpiler(혹은 compiler)와 [webpack](https://webpack.github.io/) 같은 번들러가 필요합니다.

컴포넌트에 관해서만 이야기하면 React 컴포넌트의 mixin, this autobinding 기능은 ES2015에서는 사용할 수 없습니다. 관련 기능을 이용하시려면 React.createClass를 이용해서 컴포넌트를 작성해야 합니다.(mixin 및 autobinding 기능은 재사용 컴포넌트 만들기를 설명할 때에 다시 다루도록 하겠습니다.)

#### 끝내기 전에

읽어보다 SVGElement 관련해서 잠깐 언급된 적이 있죠?

잘 되는지 한번 확인해 봐야겠습니다.

SVG 코드 안에는 Devpools 로고를 집어 넣어 보겠습니다.

https://gist.github.com/ehrudxo/203ec6a0f4c0a97236f801b7ef44c769

결과를 확인해 보니

![svg_element](http://devpools.kr/wordpress/wp-content/uploads/2016/07/svg_element-300x200.jpg)

잘 나오는 것을 확인해 볼 수 있습니다.

##### 이것이 의미 하는 것은?

UI관련 컴포넌트를 만드는 데에도 React는 잘 준비되어 있다라는 것과 아직 구조적으로 Canvas에 대한 지원은 없겠구나 라는 것을 확인할 수 있습니다.

위 내용의 코드는 모두 다음의 jsfiddle 사이트에서 확인할 수 있습니다.

<iframe src="//jsfiddle.net/ehrudxo/kwvrxpz0/embedded/" width="800" height="500" frameborder="0" allowfullscreen="allowfullscreen"></iframe>