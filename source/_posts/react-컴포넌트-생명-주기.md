---
title: REACT 컴포넌트 생명 주기
tags:
  - Lifecycle
  - React
id: 381
categories:
  - ReactJs
date: 2016-07-10 22:03:25
---

지난 번 글에 대해 피드백을 많이 주셨습니다. 너무 감사합니다.

http://devpools.kr/2016/07/03/react-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8/

### wrap-up

*   Bind 에 관련해서 다뤄봤으면 하는 부분은  조만간 다루도록 하겠습니다.
*   Stateless 함수의 Airbnb의 입장에 대해 어느 정도 갑론 을박도 이루어 지고 있어서 [Airbnb 이슈](https://github.com/airbnb/javascript/issues/937)(링크) 에 질문도 남겼습니다.  답변이 왔네요

![](https://i.imgur.com/IP033uC.jpg)

요약하자면 성능의 경우는 가장 마지막에 다뤄져야 할 부분이고 state를 다루면 다룰수록 전체 애플리케이션에 해가 될 수 (toxic) 있다고 표현 하는 군요.  조금 생각해 볼 부분입니다.

# React Component 생명 주기

React 사이트에서 정의를 따라 DOM혹은 페이지 위에 올라갈 때를 mount, DOM트리에서 삭제되거나 웹 페이지에서 없어질 때를 unmount로 정의합시다.

또한 mount 와 unmount 사이에는 여러가지 생명 주기 함수들이 동작할 것인데, state와 props는 이 함수들과 밀접하게 연관이 있습니다. props는 컴포넌트의 속성을 나타내는 변수 값을 담는 객체입니다. state는 컴포넌트 상태를 나타내는 변수 값을담는 객체입니다. 이 값들의 변화에 따라 컴포넌트는 일종의 동작을 해야 하고(생명주기 함수) 혹은 그 변화의 끝에 다시 컴포넌트를 그려줘야 할 일들이 생깁니다.(render)

다음과 같이 생명주기를 나뉘어서 컴포넌트의 상태 변화를 확인해 볼 수 있을 거 같습니다.

*   mount 될 때
*   property 가 변화될 때
*   state가 변화될 때
*   unmount 될 때
*   전체 보기

이전 React 컴포넌트를 설명할 때에

    componentDidUpdate,
    componentWillMount,
    componentWillReceiveProps,
    componentWillUnmount,
    componentWillUpdate 등은 바로 뒤에 다룰 생명주기 함수들과 연관이 있습니다
    `</pre>

    라고 이야기 했습니다. 각각의 미리 선언된 속성들을 개발자 도구에서 확인해 볼 수 있겠죠?

    ![React.createClass](/images/2016/07/React.createClass-235x300.jpg)

    그러면 실제로 React 클래스를 생성해서 컴포넌트를 Mount 를 하고 어떻게 동작하는지를 확인해 보겠습니다.

    결과가 Dracula 패턴처럼 보여지기 위해 CSS는 다음과 같이 작성하겠습니다.

    <pre>`<span class="hljs-selector-tag">body</span>{<span class="hljs-attribute">background-color</span>:<span class="hljs-number">#282a36</span>;<span class="hljs-attribute">color</span>:<span class="hljs-number">#f8f8f2</span>; <span class="hljs-attribute">font-size</span>:<span class="hljs-number">16px</span>;}
    <span class="hljs-selector-tag">body</span> <span class="hljs-selector-tag">a</span>{<span class="hljs-attribute">color</span>:<span class="hljs-number">#50fa7b</span>;<span class="hljs-attribute">padding-right</span>:<span class="hljs-number">20px</span>;}
    <span class="hljs-selector-tag">body</span> <span class="hljs-selector-tag">pre</span>{<span class="hljs-attribute">background-color</span>: <span class="hljs-number">#44475a</span>; <span class="hljs-attribute">font-weight</span>:bold;<span class="hljs-attribute">padding</span>:<span class="hljs-number">10px</span> <span class="hljs-number">10px</span> <span class="hljs-number">10px</span> <span class="hljs-number">10px</span>;}
    <span class="hljs-selector-class">.lParent</span> {<span class="hljs-attribute">height</span>:<span class="hljs-number">100px</span>; <span class="hljs-attribute">width</span>:<span class="hljs-number">90%</span>;<span class="hljs-attribute">background-color</span>:<span class="hljs-number">#6272a4</span>;<span class="hljs-attribute">margin-left</span>:<span class="hljs-number">5%</span>;}
    <span class="hljs-selector-class">.lifecycle</span> {<span class="hljs-attribute">height</span>:<span class="hljs-number">50px</span>;<span class="hljs-attribute">width</span>:<span class="hljs-number">90%</span>;<span class="hljs-attribute">background-color</span>:<span class="hljs-number">#bd93f9</span>;<span class="hljs-attribute">margin-left</span>:<span class="hljs-number">5%</span>;}`</pre>

    컴포넌트 이해를 돕기위해 Parent 컴포넌트와 LifeCycle 컴포넌트를 만들 것이기 때문에 lParent, lifecycle이라는 클래스를 생성해 두었습니다.

    HTML 코드는 다음과 같이 작성합니다.

    <pre>`<span class="hljs-tag">&lt;<span class="hljs-name">p</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"#"</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"loading"</span>&gt;</span>컴포넌트 로딩<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"#"</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"unloading"</span>&gt;</span>컴포넌트 제거<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">p</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"app"</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">pre</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"output"</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">pre</span>&gt;</span>`</pre>

    app을 id로 가지는 DOM 노드 아래에 우리가 만들 웹 컴포넌트가 들어가고, output을 id로 DOM 노드 아래에 생명주기 함수가 어떻게 호출되게 되는지를 알리기 위해 값들을 집어 넣도록 하겠습니다.

    javascript 코드는 아래와 같습니다. 명확한 이해를 위해 React.createClass로 작성했고 즉시 DOM을 변경하고 확인하는데에 jquery만큼 간단한게 없어서 사용했습니다.

    https://gist.github.com/ehrudxo/4c05664d2daf76c5346095b10a79fc97

    소스 설명을 간단하게 드리자면 컴포넌트 클래스 생성은 React.createClass 함수를 통해 작성됩니다.

    <div class="comments-icon">
    <div class="marker">
    <pre>`var 컴포넌트명 = React.createClass({...});
    `</pre>
    '컴포넌트 로딩'이라는 글자를 클릭하면 그 때에 ReactDOM.render라는 함수를 통해서 'app'이라는 id를 가진 노드에 방금 만든 컴포넌트를 로딩하는 소스입니다.

    </div>
    </div>

    <div class="marker">
    <pre>`ReactDOM.render(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">LifeCycleParent</span> /&gt;</span>, document.getElementById("app"));
    </span>`</pre>

    '컴포넌트 제거'를 클릭하면 ReactDOM.unmountComponentAtNode 함수를 통해 'app'이라는 id를 가진 노드에서 하위 컴포넌트를 모두 제거합니다.

    <div class="comments-icon">
    <div class="marker">
    <pre>`ReactDOM.unmountComponentAtNode(<span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">"app"</span>));
    `</pre>
    </div>
    <div class="marker">

    각 생명주기 함수 내의

    <div class="comments-icon">
    <div class="marker">
    <pre>`$(<span class="hljs-string">'#output'</span>).append(메세지);
    `</pre>
    </div>
    <div class="marker">

    부분은 output 을 id로 가지는 DOM 노드에 메세지를 출력하는 작업을 하고 있고, reset 함수를 통해 메세지를 모두 지울 수 있으며 그 소스는 다음과 같습니다.

    <div class="comments-icon">
    <div class="marker">
    <pre>`var reset = function(){ $('#output').empty();}
    `</pre>
    </div>
    <div class="marker">이렇게 소스를 작성하고 실행을 시키고 나면 아래와 같은 화면을 볼 수 있습니다.</div>
    <div class="marker"></div>
    <div class="marker"></div>
    <div class="marker">![LifeCycleDeepDive01](/images/2016/07/LifeCycleDeepDive01.jpg)</div>
    </div>
    </div>
    <div class="marker"></div>
    <div class="marker">

    이 정도면 눈치가 빠른 개발자라면 뭔가 의아한 부분을 발견하셨을 것이라고 생각이 듭니다. 아직 DOM에 로딩을 하기 전인데, getDefaultProps가 output에 찍혀 있네요.

    즉 ReactDOM.createClass 함수를 통해 클래스를 생성하는 시간에 getDefaultProps는 호출이 된다는 것이고 부모 컴포넌트와 자식 컴포넌트 모두 메세지를 볼 수 있네요.

    즉, 상당히 많은 블로그에서 Mounting을 설명하면서 getDefaultProps를 생명주기에 넣고 있지만, 실제로는 클래스를 만드는 작업에서 그 과정은 이루어 진다고 보는 것이 조금더 명확한 설명입니다.

    이제 조금 더 안쪽으로 들어가 보죠

    </div>

    ### 1\. mount 될 때

    <div class="marker">

    컴포넌트 로딩을 클릭하면

    <div class="comments-icon">
    <div class="marker">![LifeCycleDeepDive02](/images/2016/07/LifeCycleDeepDive02.jpg)</div>
    <div class="marker">

    와 같이 화면이 바뀝니다. 그리고 화면 아래 output 을 보면 getInitialState, componentWillMount, render, componentDidMount 순으로 호출이 되었음을 알 수 있습니다.

    정리해 보면 다음과 같습니다.

    ![mounting](/images/2016/07/mounting.jpg)

    </div>
    <div class="marker">

    ### 

    ### 2\. props가 변화될 때

    이 props 가 바뀔 때는 소스를 잘 따라 읽어보셨다면 이해가 잘 되셨을 것이지만, 아닌 사람들을 위해서 설명을 좀 더 해 보도록 하겠습니다. 위의 소스코드에서 Parent 소스를 잘 살펴보시면

    <div class="comments-icon">
    <div class="marker">
    <pre>`render(){
      <span class="hljs-keyword">return</span>(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">className</span>=<span class="hljs-string">"lParent"</span>&gt;</span>
                <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"#"</span> <span class="hljs-attr">onClick</span>=<span class="hljs-string">{this.changeProps}</span>&gt;</span>props가 바뀔때<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>&amp;nbsp;&amp;nbsp;
              <span class="hljs-tag">&lt;<span class="hljs-name">LifeCycle</span> <span class="hljs-attr">isFoobar</span>=<span class="hljs-string">{this.state.isFoobar}</span> <span class="hljs-attr">childFoobar</span>=<span class="hljs-string">{this.state.childFoobar}</span>/&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>)
    }</span>`</pre>
    </div>
    <div class="marker">

    ParentLifeCycle의 state 의 값인 isFoobar 와 childFoobar 값을 LifeCycle의 props 로 연결 시켜 놓은 것을 알 수 있습니다. 이 때 작동하는 형태는 부모 클래스의 state 값이 변하면 자식 클래스의 props가 바뀌게 되어 있는 셈입니다.

    아래 그림과 같이 생각하시면 될 듯 합니다.

    </div>

    ![LifeCycleDeepDive03](/images/2016/07/LifeCycleDeepDive03.jpg)

    <div class="marker">

    부모 클래스에서 onClick을 통해 changeProps 함수를 실행하고

    <div class="comments-icon">
    <div class="marker">
    <pre>`changeProps(){
      $(<span class="hljs-string">'#output'</span>).append(<span class="hljs-string">"\n[props가 바뀔 때]\n\n"</span>);
     <span class="hljs-keyword">this</span>.setState({
        isFoobar: <span class="hljs-literal">true</span>,
        childFoobar :<span class="hljs-literal">false</span>
      });
    },`</pre>
    </div>
    <div class="marker">

    this.setState 를 통해 state 가 바뀌면 자식 클래스인 LifeCycle의 componentWillReceiveProps 함수가 작동하게 되고 props 변경에 따라 state 변화가 자동적으로 이뤄지고 shouldComponentUpdate, componentWillUpdate, render, componentDidUpdate 등의 함수가 차례로 호출됩니다.

    <div class="comments-icon">
    <div class="marker">![LifeCycleDeepDive04](/images/2016/07/LifeCycleDeepDive04-1024x719.jpg)</div>
    <div class="marker"></div>
    <div class="marker">

    정리해 보면 다음과 같습니다.

    <div class="comments-icon">
    <div class="marker">![receivingprops](/images/2016/07/receivingprops.jpg)</div>
    <div class="marker">

    ### 

    ### 3\. state가 변화될 때

    state 가 변경될 때는 setState를 통해 state 값이 변경된 경우입니다. shouldComponentUpdate 부터 호출이 되고 아래와 같은 결과를 보여줍니다.

    </div>
    <div class="marker">![LifeCycleDeepDive05](/images/2016/07/LifeCycleDeepDive05-1024x687.jpg)</div>
    <div class="marker">

    정리해 보면 다음과 같습니다.

    <div class="comments-icon">
    <div class="marker"></div>
    <div class="marker">![changingstate](/images/2016/07/changingstate.jpg)</div>
    <div class="marker"></div>
    </div>
    <div class="marker">여기서는 shouldComponentUpdate 를 한번 살펴 봐야 하는데, 기본적으로 메쏘드를 적지 않고 놔두면 return 값은 true라 위에서 정리한 순서도를 따르지만</div>
    </div>
    </div>
    </div>
    </div>
    <div class="marker">
    <pre>`shouldComponentUpdate(){
      $(<span class="hljs-string">'#output'</span>).append(<span class="hljs-string">"shouldComponentUpdate\n"</span>);
      <span class="hljs-comment">//return true;</span>
    },`</pre>
    </div>
    <div class="comments-icon">
    <div class="marker">
    <div class="comments-icon">
    <div class="marker">
    <div class="comments-icon">
    <div class="marker">

    void 로 두거나 return false; 로 변경하면 더 이상 진행되지 않고 컴포넌트를 다시 그리지 않습니다.

    <div class="comments-icon">
    <div class="marker">![LifeCycleDeepDive06](/images/2016/07/LifeCycleDeepDive06-1024x582.jpg)</div>
    </div>
    </div>
    <div class="marker">

    ### 4\. unmount 될 때

    마지막으로 unmount 될 때를 살펴 보겠습니다.

    <pre>`ReactDOM.unmountComponentAtNode(<span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">"app"</span>));

를 통해 제거를 하면 화면에서 컴포넌트가 사라지며 componentWillUnmount 가 호출되는 것을 확인해 볼 수 있습니다

![LifeCycleDeepDive07](/images/2016/07/LifeCycleDeepDive07-1024x243.jpg)

</div>
</div>
</div>
<div class="marker">

### 5\. 전체 그림

전체 그림은 다음과 같이 확인해 볼 수 있습니다.

![part1-01](/images/2016/07/part1-01-1024x791.png)

</div>
<div class="marker">출처 : [https://staminaloops.github.io/undefinedisnotafunction/understanding-react/](https://staminaloops.github.io/undefinedisnotafunction/understanding-react/)</div>
</div>
<div class="marker">

### 지금까지 설명한 소스는 다음에서 살펴 볼 수 있습니다.

<div class="comments-icon"><iframe src="//jsfiddle.net/ehrudxo/77ub4adz/embedded/" width="800" height="500" frameborder="0" allowfullscreen="allowfullscreen"></iframe></div>
<div class="marker">[각주1]</div>
<div class="marker">

Dracula theme이라고 보통 불리우는 이 테마는 개발자들에게 가장 각광받는 에디터 테마로 어두운 배경에 개발에 도드라지는 에디터 색감을 갖고 있습니다.

깃헙 주소 : [https://github.com/dracula/dracula-theme](https://github.com/dracula/dracula-theme)

다음과 같은 Color Palette 를 가집니다.

<table>
<thead>
<tr>
<th>Palette</th>
<th>Hex</th>
<th>RGB</th>
<th>HSL</th>
<th>![Color Picker Boxes](https://draculatheme.com/assets/img/color-boxes/eyedropper.png)</th>
</tr>
</thead>
<tbody>
<tr>
<td>Background</td>
<td>`#282a36`</td>
<td>`40 42 54`</td>
<td>`231° 15% 18%`</td>
<td>![Background Color](https://draculatheme.com/assets/img/color-boxes/background.png)</td>
</tr>
<tr>
<td>Current Line</td>
<td>`#44475a`</td>
<td>`68 71 90`</td>
<td>`232° 14% 31%`</td>
<td>![Current Line Color](https://draculatheme.com/assets/img/color-boxes/current_line.png)</td>
</tr>
<tr>
<td>Selection</td>
<td>`#44475a`</td>
<td>`68 71 90`</td>
<td>`232° 14% 31%`</td>
<td>![Selection Color](https://draculatheme.com/assets/img/color-boxes/selection.png)</td>
</tr>
<tr>
<td>Foreground</td>
<td>`#f8f8f2`</td>
<td>`248 248 242`</td>
<td>`60° 30% 96%`</td>
<td>![Foreground Color](https://draculatheme.com/assets/img/color-boxes/foreground.png)</td>
</tr>
<tr>
<td>Comment</td>
<td>`#6272a4`</td>
<td>`98 114 164`</td>
<td>`225° 27% 51%`</td>
<td>![Comment Color](https://draculatheme.com/assets/img/color-boxes/comment.png)</td>
</tr>
<tr>
<td>Cyan</td>
<td>`#8be9fd`</td>
<td>`139 233 253`</td>
<td>`191° 97% 77%`</td>
<td>![Cyan Color](https://draculatheme.com/assets/img/color-boxes/cyan.png)</td>
</tr>
<tr>
<td>Green</td>
<td>`#50fa7b`</td>
<td>`80 250 123`</td>
<td>`135° 94% 65%`</td>
<td>![Green Color](https://draculatheme.com/assets/img/color-boxes/green.png)</td>
</tr>
<tr>
<td>Orange</td>
<td>`#ffb86c`</td>
<td>`255 184 108`</td>
<td>`31° 100% 71%`</td>
<td>![Orange Color](https://draculatheme.com/assets/img/color-boxes/orange.png)</td>
</tr>
<tr>
<td>Pink</td>
<td>`#ff79c6`</td>
<td>`255 121 198`</td>
<td>`326° 100% 74%`</td>
<td>![Pink Color](https://draculatheme.com/assets/img/color-boxes/pink.png)</td>
</tr>
<tr>
<td>Purple</td>
<td>`#bd93f9`</td>
<td>`189 147 249`</td>
<td>`265° 89% 78%`</td>
<td>![Purple Color](https://draculatheme.com/assets/img/color-boxes/purple.png)</td>
</tr>
<tr>
<td>Red</td>
<td>`#ff5555`</td>
<td>`255 85 85`</td>
<td>`0° 100% 67%`</td>
<td>![Red Color](https://draculatheme.com/assets/img/color-boxes/red.png)</td>
</tr>
<tr>
<td>Yellow</td>
<td>`#f1fa8c`</td>
<td>`241 250 140`</td>
<td>`65° 92% 76%`</td>
<td>![Yellow Color](https://draculatheme.com/assets/img/color-boxes/yellow.png)</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>