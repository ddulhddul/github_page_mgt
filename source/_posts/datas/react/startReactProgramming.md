---
title: 시작하세요! 리액트 프로그래밍
date: 2017-12-29 19:42:52
<!-- banner: /images/dragon_bridge.jpg -->
categories:
  - 자료정리
  - react
tags:
  - javascript
  - react
---
![/images/startReactProgramming/cover.jpg](/images/startReactProgramming/cover.jpg)
# 01. Hello World
[리액트 다운로드 URL](https://react-cn.github.io/react/downloads.html)

```js
ReactDOM.render(
  React.DOM.h1(null, 'hello world'),
  document.getElementById('app')
)
```

- **'app' div** 자리표시자의 내용이 리액트 앱에서 생성한 내용으로 대체된다.

## 작동원리
1. React 객체의 사용
  - 이 객체를 통해 모든 API에 접근할 수 있으며, API가 간소화되어있어 메서드는 그리 많지 않다.

2. ReactDOM 객체의 사용
  - **render()**가 중요하다. 여러 다른 환경(HTML, 캔버스, 안드로이드, IOS) 에서 적절하게 렌더링하는 리액트 앱을 만들 수 있다.

3. 컴포넌트 개념
  - UI 는 컴포넌트를 이용해 제작, 원하는 방법으로 이러한 컴포넌트를 조합.
  - 리액트에서는 HTML DOM을 감싸는 래퍼를 제공하는데, **React.DOM** 객체를 통해 제공
  
4. document.getElementById('app')
  - 페이지 내에서 애플리케이션의 위치를 리액트에 알려준다.
  - 이 호출은 DOM조작을 리액트의 세계로 연결하는 다리 역할을 한다.

## React.DOM.*
- React.DOM 객체를 통해 여러 html 요소를 리액트 컴포넌트로 이용할 수 있다.
- React.DOM 과 ReactDOM은 다르다. 전자는 미리 만들어진 html요소의 컬렉션, 후자는 앱을 브라우저에서 렌더링하는 방법 중 하나.
```js
React.DOM.h1({id: 'my-heading'}, "hello world")
// 첫번째 인자는 컴포넌트로 전달하려는 프로퍼티
// 두번째 인자는 컴포넌트의 자식
```

## 특수한 DOM 속성
- 반드시 알아야할 DOM 속성 **className, for, style**
- class는 자바스크립트 예약어 이므로, 대신 className, htmlFor 가 필요.
```js
React.DOM.h1({
  className: 'pretty',
  htmlFor: 'me',
  style: {
    background: 'black',
    color: 'white',
    fontFamily: 'Verdona'
  }
  //css 프로퍼티를 다룰 때, 자바스크립트 API 이름을 사용. (CamelCase)
}, "hello")
```

# 02. 컴포넌트의 수명
## 최소요건
```js
//새로운 컴포넌트를 만드는 API
var MyComponent = React.createClass({
    render: function(){
        return React.DOM.h1(null, 'hello');
    }
})

ReactDOM.render(
    React.createElement(MyComponent),
    document.getElementById('app')
)
```

- 필수요건은 **render()** 메서드를 구현하는 것. 이 메서드는 리액트 컴포넌트를 반환해야 함.

```js
ReactDOM.render(
    React.createElement('span', null, 'hello'),
    document.getElementById('app')
)
```
- React.DOM.* 메서드는 React.createElement() 를 감싸는 래퍼에 불과하다.

## 프로퍼티
- 모든 프로퍼티는 **this.props** 객체를 통해 이용할 수 있다.
- 컴포넌트를 렌더링할 때  프로퍼티를 전달하는 방법
```js
ReactDOM.render(
    React.createElement(Component, {
        name: 'ddulh'
    }, 'hello'),
    document.getElementById('app')
)
```
- this.props 는 **읽기 전용** 이라고 생각하자.

## propTypes
- 컴포넌트가 받는 프로퍼티의 목록과 형식을 선언할 수 있다.
```js
var Component = React.createClass({
    propTypes: {
        name: React.PropTypes.string.isRequired
    },
    // 기본 프로퍼티 값
    getDefaultProps: function(){
        return {
            name: ''
        }
    },
    render: function(){
        return React.DOM.span(null, 'hello'+this.props.name)
    }
})
```
- 리액트가 프로퍼티 값의 유효성을 **런타임**에 검사하므로 컴포넌트가 받는 데이터에 대해 걱정할 필요없이 render() 함수를 작성할 수 있다.
- Object.keys(React.PropTypes)

## 상태
- 리액트가 기존의 브라우저 DOM 조작과 유지 관리에 비해 가지는 장점은 애플리케이션에서 데이터가 변경될 때, 데이터의 상태에 따라 UI를 재구축한다는 점.
- render() 에서 UI를 만든 다음, 해당 데이터를 업데이트 하는데만 신경 쓰면 된다.
- 상태변경은 **this.setState()** 를 통해 한다. 호출될 경우, 리액트는 render() 메서드를 호출하고 UI를 업데이트한다.
- 항상 유효한 데이터로 작업할 수 있도록, 컴포넌트에서 getInitialState() 메서드를 구현
```js
getInitialState(){
    return {
        name: this.props.name
    }
}
```

## DOM 이벤트 참고사항
- 리액트는 성능과 편의성, 유효성 유지를 위해 자체 합성 이벤트 시스템을 이용
- 브라우저 불일치 문제를 자연스럽게 해결.
- 또한 이벤트를 취소하는 API가 모든 브라우저에서 동일
  - event.stopPropagation() 과 event.preventDefault() 가 이전 IE에서도 작동
- 리액트는 이벤트핸들러에 캐멀표기법을 이용하므로 ON-CLICK 을 이용해야 한다.

## 프로퍼티와 상태
- this.props: 프로퍼티는 바깥환경에서 컴포넌트를 구성하기 위한 매커니즘
- this.state: 상태를 컴포넌트의 내부 데이터

## 외부에서 컴포넌트 접근
```js
var myTextAreaCounter = ReactDOM.render(
    React.createElement(TextAreaCounter, {
        defaultValue: 'ddulh'
    })
)

// 새로운 상태 설정 - 외부에서의 상태 변경은 가급적 권장되지 않음
myTextAreaCounter.setState({text: 'Hello ~~~'})
// 리액트가 생성한 주 부모 DOM 노드에 대한 참조를 얻기
var reactAppNode = ReactDOM.findDOMNode(myTextAreaCounter)
// 프로퍼티와 상태에 접근
myTextAreaCounter.props;
myTextAreaCounter.state;
```

## 수명 주기 메서드
```js
var logMixin = {
    _log: function(methodName, args){
        console.log(this.name+ '::'+methodName, args)
    },
    componentWillUpdate: function(){
        // 컴포넌트의 render() 메서드가 호출되기 전에 실행
        this._log('componentWillUpdate', arguments)
    },
    componentDidUpdate: function(){
        // render() 메서드가 작업을 완료하고 기반 DOM의 새로운 변경사항이 적용된 후에 실행
        this._log('componentDidUpdate', arguments)
    },
    componentWillMount: function(){
        // 노드가 DOM에 삽입되기 전에 실행
        this._log('componentWillMount', arguments)
    },
    componentDidMount: function(){
        // 노드가 DOM에 삽입된 후에 실행
        this._log('componentDidMount', arguments)
    },
    componentWillUnmount: function(){
        // 컴포넌트가 DOM에서 제거되기 직전에 실행
        this._log('componentWillUnmount', arguments)
    },
    shouldComponentUpdate: function(newProps, newState){
        // componentWillUpdate() 가 호출되기 전에 실행되며 return false; 를 통해 업데이트를 취소할 수 있다. (render가 실행되지 않게)
        this._log('shouldComponentUpdate', arguments)
    },
}
```

## 믹스인 사용
- 믹스인은 메서드와 프로퍼티의 컬렉션을 포함하는 자바스크립트 객체
- 다른 객체의 프로퍼티에 포함해서 사용한다.

```js
var TextAreaCounter = React.createClass({
    name: 'TextAreaCounter',
    mixins: [logMixin],
    // 나머지 부분..
})
```

## 자식 컴포넌트 사용
- 자식 컴포넌트가 부모 컴포넌트보다 먼저 마운팅 되고 업데이트 된다.

## 성능을 위한 컴포넌트 업데이트 방지
- 순수 컴포넌트: 자신의 render() 메서드에서 this.props 와 this.state만 사용하며 추가 함수 호출을 하지 않는 컴포넌트 클래스
- 이러한 컴포넌트에서 shouldComponentUpdate() 를 구현하여 프로세싱 파워를 절약 할 수 있다.
```js
shouldComponentUpdate(nextProps, newState){
    return nextProps.count !== this.props.count;
}
```

## PureRenderMixin
```js
<script src="react/build/react-with-addons.js"></script>
var Counter = React.createClass({
    name: 'Counter',
    mixins: [React.addons.PureRenderMixin],
    //...
})
```
- this.props와 nextProps 를 비교하고 this.state와 nextState를 비교하는 작업은 항상 해야하므로...
- 범용으로 쓸수 있는 **React.addons.PUreRenderMixin** // 리액트 애드온의 확장 버전에 포함돼 있다.

# 03. Excel: 멋진 테이블 컴포넌트
## [데이터 준비]
[https://github.com/stoyan/reactbook/blob/master/chapters/03.01.table-th.html](https://github.com/stoyan/reactbook/blob/master/chapters/03.01.table-th.html)

## 테이블 헤더 루프
```js
var Excel = React.createClass({
    render: function(){
        return (
            React.DOM.table(null,
                React.DOM.thead(null,
                    React.DOM.tr(null,
                        this.props.headers.map(function(title){
                            return React.DOM.th(null, title)
                        })
                    )
                )
            )
        )
    }
})
```

# 04. JSX
## Hello JSX
```js
ReactDOM.render(
    <h1 id='my-heading'>
        <span><em>Hell</em>o</span> world!
    </h1>
)
```

- 유효한 자바스크립트 구문이 아니기 때문에, 이 상태로 브라우저에서 실행할 수는 없다. 순수 자바스크립트로 변환(트랜스파일) 해야한다.

## 바벨
- 오픈소스 트랜스파일러
- 브라우저 내(클라이언트측) 변환을 수행하려면 browser.js 파일이 필요. 바벨6버전 이후로는 제공하지 않지만

```linux
$ mkdir ~/reactbook/babel
$ cd ~/reactbook/babel
$ curl https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js > browser.js
```

## 클라이언트 측
- JSX 트랜스파일을 처리하는 스크립트인 browser.js 포함
- 바벨이 작업할 부분을 알 수 있도록 JSX를 이용하는 script 태그에 마크업 추가
```html
<script src="/babel/babel.js"></script>
<script type="text/babel"> ... </script>
```

## JSX 변환
- [https://babel.js.io/repl/](https://babel.js.io/repl/)
- [https://facebook.github.io/react/html-jsx.html](https://facebook.github.io/react/html-jsx.html)

## JSX 에서 자바스크립트 사용
- 간단하게 중괄호 안에 자바스크립트 코드를 넣으면 된다.
```js
<tr key={idx}>
    {
        row.map(function(cell, idx){
            return <td key={idx}>{cell}</td>
        })
    }
</tr>
```

## JSX 의 주석
```js
<h1>
    {/* asd */}
    {
        // asd
    }
</h1>
```

## XSS 방지
- HTML 엔터티를 곧바로 사용하지 않는 이유는 XSS 공격을 방지하기 위해서다.
- 리액트는 XSS 공격을 방지하기 위해 모든 문장열을 이스케이프한다.

## 스프레드 속성
```js
var attr = {
    href: 'http://naver.com',
    target: '_blank'
}

return (
    <a {...attr}>Hello</a>
)
```

## 부모 대 자식 스프레드 속성
```js
return (
    <a {...this.props}>{this.props.children}</a>
)
```

## JSX 에서 여러 노드 변환
- render() 함수에서는 항상 단일 노드를 반환해야 한다.
```js
var Example = React.createClass({
    render: function(){
        console.log(this.props.children.length)
        return (
            <div>
                {this.props.children}
            </div>
        )
    }
})
ReactDOM.render(
    <Example>
        <span key='great'>Hello</span>
        {' '}
        <span key='word'>World</span>
        !
    </Example>,
    document.getElementById('app')
)
```

## JSX와 HTML의 차이점
1. className 과 htmlFor
2. style이 객체로 취급됨
  ```js
  var style = {
      fontSize: '2em',
      lineHeight: '1.6'
  }
  var em = <em style={style} />;
  ```
3. 닫는 태그
  - HTMl 태그 중에는 닫을 필요가 없는 것이 있지만, JSX(XML) 에서는 항상 닫아야 한다.
4. 캐멀표기법으로 속성표기
  - 이 규칙의 예외는 data-와 aria- 접두사가 붙은 속성이며, 이들 속성은 HTML과 동일하게 사용된다.

## JSX와 폼
1. ON-CHANGE 핸들러
  - 리액트에서는 ON-CHANGE 속성을 이용해 변경 이벤트를 구독할 수 있다.
  - 라디오 버튼이나 체크박스의 checked 값이나 selected 값을 이용하는 것보다 훨씬 일관성 있다.
2. value 와 defaultValue
  - input 의 내용을 변경하면 value는 값이 바뀌지만, defaultValue는 유지된다.
3. textare 태그와 value
4. select 태그와 value
  ```html
  <select defaultValue={{"stay", "move"}} multiple={true}>
      <option value="stay">Stay</option>
      <option value="move">Move</option>
  </select>
  ```

## JSX를 이용한 Excel 컴포넌트 수정
[https://github.com/stoyan/reactbook/](https://github.com/stoyan/reactbook/)

# 05. 앱 개발을 위한 설정
## 기본 파트 앱
1. 파일과 폴더
  - /css, /js, /images 폴더와 모두 연결할 index.html 파일 필요
  - 스크립트와 JSX 구분을 위한 /js/sourece/ 폴더와 브라우저 친화적인 스크립트를 위한 /js/build/  폴더가 필요
  - 빌드를 수행할 명령줄 스크립트를 호스트하는 /scripts 카테고리
2. index.html
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <title>App</title>
      <meta charset="UTF-8">
      <!-- 모든 CSS를 포함하는 단일 bundle.css 파일 -->
      <link rel="stylesheet" type="text/css" href="bundle.css">
  </head>
  <body>
      <!-- 앱이 시작할 위치 -->
      <div id='app'></div>
      <!-- 모든 자바스크립트를 포함하는 단일 bundle.js 파일 -->
      <script src="bundle.js"></script>
  </body>
  </html>
  ```

3. CSS
  ```css
  html {
    background: white;
    font: 16px Arial;
  }
  ```

## 모듈
- 널리 채용되고 있는 개념으로 CommonJS 가 있다.
- 작업이 완료되면 하나 이상의 심볼을 내보내는 파일에 코드를 포함한다.
- 모듈 하나가 항목 하나(리액트 컴포넌트 하나)를 내보내도록 하는 것이 관행이다.

## ECMAScript 모듈
- ECMAScript 사양은 이 개념을 발전시켜, require() 와 module.exports 에 의존하지 않는 새로운 구문을 제안한다.
```js
// source/app.js
'use strict';

import React from 'react';
import ReactDOM from 'react-dom';
import Logo from './components/Logo';

ReactDOM.render(
    <h1>
        <Logo /> Welcome to the app!
    </h1>,
    document.getElementById('app')
)
```

```js
// source/components/Logo.js
import React from 'react';

class Logo extends React.Component{
    render(){
        return <div className="Logo" />;
    }
}
export default Logo
```

## 필수 구성 요소 설치
- index.html 을 로드하고 작동하는지 확인하려면
  - bundle.css 를 만든다.
  - 브라우저가 코드를 이해할 수 있게 만든다 (트랜스파일링을 위해 바벨이 필요)
  - vundle.js 를 만든다. Browserify를 이용
    - 모든 종속성을 확인후 포함한다. (리액트, Logo.js 등)
    - require() 호출이 작동하도록 CommonJS 구현을 포함한다.
    - 바벨은 모든 import문을 require() 함수 호출로 변환한다.

```bash
$ npm install --global browserify babel-cli
```

## 리액트 및 기타 항목
- 패키지를 로컬로 설치
  - react
  - react-dom
  - babel-preset-react // 바벨에 JSX 지원 및 다른 리액트 기능을 제공
  - babel-preset-es2015 // 최신 자바스크립트 기능에 대한 지원을 제공
  ```bash
  $ npm install --save-dev react react-dom babel-preset-react babel-preset-es2015
  ```

## 빌드 시작
빌드 프로세스는 **CSS 연결, JS 트랜스파일, JS패키징** 의 세 단계로 수행
- 자바스크립트 트랜스파일
```bash
## js/source 의 모든 파일을 리액트와 ES2015 기능을 이용해 트랜스파일한 후 결과는 js/build 로 복사
$ babel --presets react,es2015 js/source -d js/build
```

- 자바스크립트 패키징
```bash
## app.js 부터 시작해 모든 의존성을 찾고 결과를 bundle.js 로 기록하도록 명령
$ browserify js/build/app.js -o bundle.js
```

- CSS 패키징
```bash
$ cat css/*/* css/*.css | sed 's/..\/..\/images/images/g' > bundle.css
```

## 윈도우 버전
디렉터리 분리 문자가 다르다는 점을 주의
```bash
$ babel --presets react,es2015 js\source -d js\build
$ browserify js\build\app.js -o bundle.js
$ type css\components\* css\* > bundle.css
```

## 개발 중 빌드하기
디렉터리에 대한 변경 사항을 확인하고 스크립트를 자동 빌드
- scripts/build.sh
```sh
# js 트랜스폼
babel --presets react,es2015 js\source -d js\build
# js 패키징
browserify js\build\app.js -o bundle.js
# css 패키징
cat css/*/* css/*.css | sed 's/..\/..\/images/images/g' > bundle.css
# 완료
date; echo;
```

## 배포
JS 축소기인 uglify-js 와 CSS 축소기 cssshrink 를 이용
```sh
# 마지막 버전을 정리
rm -rf __deployme
mkdir __deployme

# 빌드
sh scripts/build.sh

# 축소
uglifyjs bundle.js -o __deployeme/bundle.js
cssshrink bundle.css > __deployme/bundle.css

# HTML 과 이미지 복사
cp index.html __deployme/index.html
cp -r images/ __deployme/images/

# 완료
date; echo;
```


# 07. 린트, 플로우, 테스트 반복
## package.json
### 바벨, 스크립트 구성
```js
{
  "name": "whinepad",
  "version": "2.0.0",
  "babel" : {
    "presets":[
      "es2015",
      "react"
    ]
  },
  "scripts":{
    "build": "sh scripts/build.sh"
  }
}
```

## ESLint
- 잠재적 위험이 될 수 있는 패턴을 검색

### 설정
```bash
$ npm i -g eslint eslint-plugin-react eslint-plugin-babel

//package.json
{
  "eslintConfig": {
    "parser": "babel-eslint",
    "plugin": [
      "babel",
      "react"
    ],
    "env": {
      "browser": true
    },
    "rules": {
      "react/jsx-uses-react": 1,
      "comma-dangle": [2, "always-multiline"]
    }
  }
}
```

### 실행
```bash
$ eslint js/source
```

## 플로우
- 자바스크립트용 정적 형식 검사기
- /* @flow */
```bash
$ npm install -g flow-bin
$ cd ~/reactbook/whinepad2

## .flowconfig
[ignore]
.*/react/node_modules/.*

[include]
node_modules/react
node_modules/react-dom
node_modules/classnames

[libs]

[options]

```

## 실행
```bash
$ flow
```