# React Native Basic

## JSX & export

~~~javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class HelloWorldApp extends Component {
  render() {
    return (
      <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
        <Text>Hello, world!</Text>
      </View>
    );
  }
}     
~~~

<br/>

위의 예제 코드에서 export 부분을 자세히 살펴보겠다.

<br/>

~~~javascript
<View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
  <Text>Hello, world!</Text>
</View>
~~~

<br/>

위의 코드는 JSX 형식으로 자바스크립트 안에 XML을 포함시키는 형식이다. 일반적인 웹(web)에서의 \<div>, \<span> 대신 react-native에서는 \<view>, \<Text>를 사용한다. \<Text>는 텍스트를 Built-in 하는 컴포넌트이며, \<view>는 웹(web)에서의 \<div>, \<span> 역할을 한다.

다음과 같이 JSX를 포함하고 있는 export default class인 HelloWorldApp은 임의로 만들어 준 하나의 컴포넌트이다. 저런식으로 컴포넌트를 만들어서 이어붙이는 것이고, 컴포넌트 생성 시 render 함수만 포함해주면 된다. render 안에는 JSX가 들어간다.

<br/>

## export

ES6의 module 문법 표준에 따르면 export는 2가지 종류가 있다. named exports(모듈당 여러 개의 export)와 default exports(모듈당 하나의 export)가 존재한다.

react-native에서는 대부분(?) default exports를 사용하고 있는 것으로 알고 있다.

### named exports

하나의 module 파일이라도 이 중 일부만 필요로 하는 경우가 있을 수 있다. 예를 들어 하나의 함수 혹은 하나의 클래스만 사용하고 싶은 경우이다. 이런 때 es6 module 문법이 이를 가능하게 한다.

<br/>

~~~javascript
//------------ lib.js ------------
export const sqrt = Math.sqrt;

export function square(x) {
    return x * x;
}

export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
  
//------------ main.js ------------
import { square, diag } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5

//------------ main.js ------------
//(위와 다른 방법)
import * as lib from 'lib';
console.log(lib.square(11)); // 121
console.log(lib.diag(4, 3)); // 5
~~~

<br/>

### default exports

nodeJS를 이용하여 개발을 진행하는 경우 변수 하나만 export 하는 방식을 빈번히 사용하게 된다. 이때 constructor나 class를 export 하는 경우가 많다. 이런 경우 하나의 module이 하나의 export만 갖는 경우가 되는데, 이런 때 사용할 수 있는 것이 default exports 이다.

<br/>

~~~javascript
//------------ myFunction.js ------------
export default function () { ... };

//------------ main1.js ------------
import myFunc from 'myFunc';
myFunc();
~~~

~~~javascript
//------------ myClass.js ------------
export default function () { ... };

//------------ main2.js ------------
import myClass from 'myClass';
let inst = new myClass();
~~~

<br/>

## Component

react-native 에서는 기존의 react나 웹에서와 같이 div등 과 같은 컴포넌트가 존재하지 않고, 특별하게 View, Text와 같은 컴포넌트가 존재한다.

즉, react-native 에서는 한정된 컴포넌트를 사용할 수 있다.

react-native 에서는 Stylesheet, View, Text 컴포넌트를 사용하고, 이런 특정 컴포넌트는 각 모바일 환경에 맞춰서 자동적으로 변환된다. iOS의 경우에는 Objective-C, 안드로이드의 경우는 Java로 각각의 컴포넌트가 네이티브하게 변환된다.

<br/>

## render

컴포넌트를 생성하면 기본적으로 존재해야 하는 함수이며, 컴포넌트의 UI에 대한 설명을 반환하는 기능을 한다. HTML DOM을 렌더링하여 사용자에게 보여주는 역할을 한다. 여기에서 HTML DOM은 가상 DOM을 의미하며, 컴포넌트가 마운트될 때나 state에 변화가 일어나 DOM의 리렌더링이 필요할 때 호출하게 된다. state는 컴포넌트의 속성 중 하나이다.

<br/>

## Style

react-native에서는 Style이라는 객체를 만들어 사용한다. Style 객체는 StyleSheet.create()를 사용하여 flex를 적용하는 것이 특징이다.

화면 UI 레이아웃을 잡기 위해 웹 등에서 매우 편하게 사용하는 flex를 이제 react-native를 통해 모바일 애플리케이션에서도 사용할 수 있다.

<br/>

~~~javascript
export default class StyleTest extends Component {
    // render 함수
    render() { 
        return (
            // View 태그에 style 설정을 하고, 아래 const style에 저장한 StyleSheet.create*() 함수를 통해서 만든 Style을 적용한다.
            <View style={style.container}>
                <Text>StyleTest</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
      flex: 1,
      alignItems: 'center',
      justifyContent: 'center',
    },
});
~~~

<br/>

## FlexBox

flez 속성은 부모 뷰 컴포넌트에서 먼저 속성을 정의한다. 그리고 자식 뷰 컴포넌트에서 확장(expend)을 하는 구조이다. 부모가 속성(예, flex: 1)을 정의할 수 있고, 여기서 1은 비율을 의미한다.

### flexDirection

~~~javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class FlexDirectionBasics extends Component {
  render() {
    return (
      // Try setting `flexDirection` to `column`.
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('Hello', () => FlexDirectionBasics);
~~~

<br/>

flexDirection의 경우 자식 뷰 컴포넌트의 방향을 의미한다. flexDirection을 row로 설정하면 자식 뷰 컴포넌트의 확장이 가로 방향으로 진행된다. 위의 예제의 경우 자식 뷰 컴포넌트는 width, height 속성을 지정함으로써 크기가 고정임을 알 수 있다.

<br/>

## Justify Content

부모 뷰 컴포넌트에서 jusifyContent 속성을 정의하여 자식 뷰 컴포넌트의 배치에 영향을 줄 수 있다. 부모 뷰 컴포넌트에서 justifyContent 속성으로 flex-start, center, flex-end, space-around, space-between 등을 가질 수 있다. 자식 뷰 컴포넌트의 간격에 영향을 준다.

[참고 링크](https://naradesign.github.io/article/flex-justify-align.html)

<br/>

~~~javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class JustifyContentBasics extends Component {
render() {
  return (
    // Try setting `justifyContent` to `center`.
    // Try setting `flexDirection` to `row`.
    <View style={{
      flex: 1,
      flexDirection: 'column',
      justifyContent: 'space-between',
    }}>
      <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
      <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
      <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
    </View>
    );
  }
};

AppRegistry.registerComponent('Hello', () => JustifyContentBasics);
~~~

<br/>

## Align Items

~~~javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class AlignItemsBasics extends Component {
  render() {
    return (
      // Try setting `alignItems` to 'flex-start'
      // Try setting `justifyContent` to `flex-end`.
      // Try setting `flexDirection` to `row`.
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('Hello', () => AlignItemsBasics);
~~~

<br/>

alignItems 속성은 자식 뷰 컴포넌트의 보조 축(secondary axis)의 정렬 기준을 결정한다.

예를 들어서 flexDirection 이 column 이면 세로 방향이 주축(primary axis)이고 가로 방향이 보조 축(secondary axis)이다. 여기서는 가로 방향에 대해서 가운데 정렬을 하게 된다.

사용 가능한 값으로 flex-start, center, flex-end stretch 가 있다.

이외에도 참고할만한 레이아웃 디자인 요소 : [레이아웃 참고 링크](https://yuddomack.tistory.com/entry/5React-Native-%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83-%EB%94%94%EC%9E%90%EC%9D%B8-1%EB%B6%80-flex%EC%99%80-width-height)

<br/>

## Props

### Props란?

properties의 약어이며, 컴포넌트를 생성할 때, 파라미터를 받아서 생성하는 경우가 많은데, 이렇게 받아온 데이터를 사용하는 것을 말한다. 변동되지 않는 데이터를 다룰 때 사용한다. 부모에 의해 설정되며, 컴포넌트가 살아있는 동안만 유지된다.

### React Native 제공 컴포넌트

리액트 네이티브의 기본 컴포넌트인 image 예시이다.

<br/>

~~~javascript
import React, { Component } from 'react';
import { Image } from 'react-native';

export default class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}}/>
    );
  }
}
~~~

<br/>

~~~javascript
<image source={pic} ... >
~~~

<br/>

위의 태그에서 source가 prop 이름이다. {pic}부분은 변수 pic을 JSX 형식으로 써 준 것이다.

<br/>

### Customize Component

리액트 네이티브에서 기본적으로 제공하는 컴포넌트 말고, 직접 컴포넌트를 만들 때도 prop를 사용할 수 있다. render 함수 안에 this.props로 사용할 수 있다.

<br/>

~~~javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Text>Hello {this.props.name}!</Text>
      </View>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center', top: 50}}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}
~~~

<br/>

위에서 만든 Greeting 컴포넌트를 아래의 LotsOfGreetings 컴포넌트에서 사용하고 있다. LotsOfGreetings의 render안에 Greeting 컴포넌트가 3번 사용되고 있다. 각각 Greeting 컴포넌트는 'name' 이라는 prop를 가지고 있다. 이 prop 값을 Greeting 컴포넌트에서 사용하기 위하여, {this.props.name}을 사용하고 있는 것이다.

출력 결과는 아래와 같다. 각각 Greeting의 prop 값에 따라 출력 값이 달라진다.

~~~
Hello Rexxar!
Hello jaina!
Hello Valeera!
~~~

<br/>

## State

### State란?
 
데이터를 컨트롤하는 방법에는 2가지가 있다. 하나는 앞에서 언급한 props이고 다른 하나는 state이다. prop와 달리 state는 변화하는 데이터를 다룰 때 사용한다.

### State 사용방법

생성자에 state를 초기화 해주고, 변화가 생길 때 마다, setState를 불러 업데이트를 한다.

### Prop와 State

예를들어 계속 깜빡거리는 어떤 text가 있다고 한다면, text의 문구 자체는 변하지 않으므로 prop이다. 그러나 깜빡거리는 상태는 계속 변하므로 State이다.

<br/>

~~~javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Blink extends Component {

  componentDidMount() {
    // Toggle the state every second
    setInterval(() => (
      this.setState(previousState => (
        { isShowingText: !previousState.isShowingText }
      ))
    ), 1000);
  }

  //state object
  state = { isShowingText: true };

  render() {
    if (!this.state.isShowingText) {
      return null;
    }

    return (
      <Text>{this.props.text}</Text>
    );
  }
}

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}
~~~

<br/>

setState가 호출되면, BlinkApp이 컴포넌트를 re-render 할 것이다. 위의 예시에서는 1초마다 re-render해준다. 즉, 처음에 isShowingText를 true로 설정해주고, 1초마다 이전 값의 반대값을 할당해준다. true일 경우는 text를 출력해주고, false일 경우, text를 출력해주지 않기 때문에 text가 깜빡거릴 것이다.

<br/>

## ReactNative - Life Cycle

<br/>

~~~javascript
import React, {Component} from 'react';
... (중략) ...
export default class App extends Component{
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}
... (중략) ...
~~~

<br/>

Component는 react의 class, interface를 참조한다.

<br/>

~~~javascript
import React, {Component} from 'react';
...
~~~

<br/>

해당 라이브러리의 소스를 확인해보면, 아래와 같은 Lifecycle에 관련된 소스를 확인할 수 있다.

<br/>

~~~javascript
... (중략) ...
interface Component<P = {}, S = {}, SS = any> extends ComponentLifecycle<P, S, SS> { }
... (중략) ...
//
// Component Specs and Lifecycle
// ---------------------------------------------------------------
// This should actually be something like `Lifecycle<P, S> | DeprecatedLifecycle<P, S>`,
// as React will _not_ call the deprecated lifecycle methods if any of the new lifecycle
// methods are present.
interface ComponentLifecycle<P, S, SS = any> extends NewLifecycle<P, S, SS>, DeprecatedLifecycle<P, S> {
/**
* Called immediately after a component is mounted. Setting state here will trigger re-rendering.
*/
componentDidMount?(): void;
/**
* Called to determine whether the change in props and state should trigger a re-render.
* `Component` always returns true.
* `PureComponent` implements a shallow comparison on props and state and returns true if any
* props or states have changed.
* If false is returned, `Component#render`, `componentWillUpdate`
* and `componentDidUpdate` will not be called.
*/
shouldComponentUpdate?(nextProps: Readonly<P>, nextState: Readonly<S>, nextContext: any): boolean;
/**
* Called immediately before a component is destroyed. Perform any necessary cleanup in this method, such as
* cancelled network requests, or cleaning up any DOM elements created in `componentDidMount`.
*/
componentWillUnmount?(): void;
/**
* Catches exceptions generated in descendant components. Unhandled exceptions will cause
* the entire component tree to unmount.
*/
componentDidCatch?(error: Error, errorInfo: ErrorInfo): void;
}
... (중략) ...
~~~

위 소스는 Component 인터페이스가 ComponentLifecycle를 상속받고 있는 부분과 ComponentLifecycle의 Interface 상세 부분이고, ComponentLifecycle Interface는 NewLifecycle 과 DeprecatedLifecycle의 Interface를 상속받아져 있는 것을 확인 할 수 있다.

~~~javascript
interface ComponentLifecycle<P, S, SS = any> extends NewLifecycle<P, S, SS>, DeprecatedLifecycle<P, S> { ...
~~~

<hr/>

React Native의 라이프 사이클은 크게 5가지 정도가 있다.
React 16.3 부터는 ..will.. 시점의 메소드는 사용을 지양하고 있으며, React 17부터는 사용이 불가능하도록하고 있다.

#### 1. 컴포넌스 생성부터 완료의 호출 순서를 보면

constructor ➡️ componetWillMount(depricated) ➡️ render ➡️ componetDidMount

#### 2.prop 변화가 있을 경우 호출 순서를 보면

componentWillReceiveProps(depricated) ➡️ shouldComponetUpdate (false 시 업데이트 취소) ➡️ componetWillUpdate(depricated) ➡️ render ➡️ componetDidUpdate

#### 3.state변화가 있으면

shouldComponetUpdate (false 시 업데이트 취소) ➡️ componetWillUpdate(depricated) ➡️ render ➡️ componetDidUpdate

#### 4.컴포넌트를 제거하면

componentWillUnmount

#### 5. 구성요소에 문제가 있을 경우

componentDidCatch

<hr/>

위 메소드에 정의된 ComponentLifecycle, NewLifecycle, DeprecatedLifecycle 의 정보를 바탕으로 아래에 정리해 보았다.

### ComponentLifecycle

> componentDidMount(): void;

- 컴포넌트가 마운트 된 직후에 불려지게 된다
- 여기에 상태를 설정하면 다시 렌더링이 시작된다

> shouldComponentUpdate(nextProps: Readonly<P>, nextState: Readonly\<S>, nextContext: any): boolean;

- props와 states의 변경으로 다시 렌더링을 트리거 해야하는지 여부를 결정하기 위해 호출된다
- props 또는 states 가 바뀌었을 때 호출된다
- false가 반환되면, render, componentWillUpdate, compoㅍnentDidUpdate는 호출되지 않는다

> componentWillUnmount(): void;

- 컴포넌트가 제거(파기)되기 직전에 불려지게 된다
- componentDidMount 에서 생성된 DOM 요소 정리와 같이.. 이 메소드에서는 필요한 것을 정리하는 역활을 한다.

> componentDidCatch(error: Error, errorInfo: ErrorInfo): void;

- 하위 구성 요소에서 생성된 예외가 있을 경우 호출된다
- 처리되지 않은 예외로 인해 전체 구성 요소 트리가 마운트 해제된다.

### NewLifecycle

> getSnapshotBeforeUpdate(prevProps: Readonly\<P>, prevState: Readonly\<S>): SS | null;

- render의 결과를 적용하기 전에 실행된다.
- componentDidUpdate에 제공할 객체를 반환된다.
- render가 변경되기 전에 스크롤 위치와 같은 것을 저장하는데 유용하다.
- 참고 : getSnapshotBeforeUpdate 가 있으면 더 이상 사용되지 않는 라이프 사이클 이벤트가 실행되지 않습니다.

> componentDidUpdate(prevProps: Readonly\<P>, prevState: Readonly\<S>, snapshot?: SS): void;

- 업데이트가 발생한 직후에 호출된다. 단, 초기 렌더링에는 호출되지 않는다.
- 파라미터 중에 snapshot은 getSnapshotBeforeUpdate 가 있는 경우에만 존재하며 null이 아닌 것을 반환합니다.

### DeprecatedLifecycle

아직 사용할 수 있지만, 앞으로 사용되지 않을 라이프사이클 메소드이다. Lifecycle에서 will에 해당하는 메소드는 앞으로 지양하려하는 것을 알 수 있다. 여기의 메소드는 이제 사용을 자제하거나 안하는 것이 좋을 것으로 예상된다.

> componentWillMount(): void;

- react 16.3 에 deprecated 되었고, react 17부터는 사용할 수 없다.
- render가 시작 되기 전에 호출되며, 마운트 되기 전에 호출된다.
- 이 방법을 통해 side-effects 또는 subscriptions를 피할 것을 권장한다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않는다.

> UNSAFE_componentWillMount(): void;

- react 16.3 에 deprecated 되었고, 대신에 componentDidMount 나 constructor에서 사용하길 권장하며, react 17부터는 사용할 수 없다.
- render 가 시작 되기 전에 호출되며, 마운트 되기 전에 호출된다.
- 이 방법을 통해 side-effects 또는 subscriptions를 피할 것을 권장한다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않다.

> componentWillReceiveProps(nextProps: Readonly<P>, nextContext: any): void;

- react 16.3에 deprecated되었고, 대신에 static getDerivedStateFromProps을 사용하길 권장하며, react 17부터는 사용할 수 없다.
- setState가 호출되면 실행되지 않는다.
- 새로운 props를 받을 때 호출된다.
- 새 props와 기존의 props를 비교할 수 있다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않는다.

> UNSAFE_componentWillReceiveProps(nextProps: Readonly\<P>, nextContext: any): void;

- react 16.3에 deprecated 되었고, 대신에 static getDerivedStateFromProps을 사용하길 권장한다.
- setState가 호출되면 실행되지 않는다.
- 새로운 props를 받을 때 호출된다.
- 새 props와 기존의 props를 비교할 수 있다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않는다.


> componentWillUpdate(nextProps: Readonly\<P>, nextState: Readonly\<S>, nextContext: any): void;

- react 16.3에 deprecated 되었고, 대신에 getSnapshotBeforeUpdate 을 사용하시길 권장한다고 하며, react 17부터는 사용할 수 없다.
- 새로운 Props 또는 state가 수신하여 렌더링 직전에 호출된다. 초기 렌더링에는 호출되지 않는다.
- 주의! 여기에서 setState 를 하시면 안된다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않는다.

> UNSAFE_componentWillUpdate(nextProps: Readonly\<P>, nextState: Readonly\<S>, nextContext: any): void;

- react 16.3에 deprecated 되었고, 대신에 getSnapshotBeforeUpdate을 사용하시길 권장한다.
- 새로운 Props 또는 state가 수신하여 렌더링 직전에 호출된다. 초기 렌더링에는 호출되지 않는다.
- 주의! 여기에서 setState 를 하시면 안된다.
- 참고 : getSnapshotBeforeUpdate 또는 getDerivedStateFromProps가 있으면 호출되지 않는다.


그리고 위 라이프 사이클에는 없지만,

> constructor

- 생성자 메소드입니다. 컴포넌트가 처음 만들어질 때 실행된다.

> render

- 컴포넌트를 만드는 방법을 제시하고 만들기 시작한다.
