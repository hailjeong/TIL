# LifeCycle(라이플 사이클: 수명주기)

- 모든 리액트 컴포넌트에는 라이프사이클이 존재한다.
- 컴포넌트의 수명은 페이지에 렌더링 괴지 전인 준비 과정에서 시작하여 페이지에서 사라질 때 끝난다.
- 처음으로 렌더링할 때 어떤 작업을 처리해야 하거나 컴포넌트를 업데이트 하기 전후로 어떤 작업을 처리해야 할 수도 있고, 불필요한 업데이트를 방지해야 할 수도 있다.
- 라이크 사이클 메서드는 클래스형 컴포넌트에서만 가능하며 함수형 컴포넌트에서는 Hooks 기능중에 useEffect를 사용한다. 

## LifcCycle 이해하기
- 라이프사이클을 메서드의 종류는 총 9가지 이다.
- Will 접두가가 붙은 메서드는 어떤 작업을 작동하기 전❗️ 에 실행되는 메서드이다.
- Did 접두사가 붙은 메서드는 어떤 작업을 작동한 후❗️ 에 실횅되는 메서드이다.
- 라이프사이클은 총 3가지이다. 마운트(페이지에 컴포넌트가 나타남) --> 업데이트(컴포넌트 정보를 업데이트) --> 언마운트(페이지에서 컴포넌트가 사라짐)로 나뉜다.

## 마운트(Mount)
- DOM이 생성되고 웹 브라우저상에 나타나는 것을 마운트(Mount)라고 한다.
- 이때, 호출하는 메서드는 다음과 같다.
- 컴포넌트 만들기 --> constructor(컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드) --> getDerivedStateFromProps(props에 있는 값을 state에 넣을 때 사용되는 메서드) --> render(준비한 UI를 렌더링하는 메서드) --> componenDidMount(컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드)

## 업데이트(Update)
- 컴포넌트는 4가지의 경우에 업데이트를 한다.
  - props가 바뀔 때
  - state가 바뀔 때
  - 부모 컴포넌트가 리렌더링 될 때
  - this.forceUpdate로 강제로 렌더링을 트리거 할 때
- 업데이트 할 때는 다음처럼 하면 된다.
- 업데이트를 발생시키는 이유 --> getDerivedStateFromProps(마운트에서도 호출되며, 업데이트 시작하기 전에도 호출된다. props의 변화에 따라 state값에도 변화를 주고 싶을 때 사용한다.) --> shouldComponentUpdate(컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드. true 혹은 false를 반환해야 하며, true를 반환하면 다음 라이프사이클 메서드르 계속 실행하고, false를 반환하면 작업을 중지한다. 컴포넌트가 리렌더링되지 않는다. 만약 this.forceUpdate() 함수를 호출하면 앞에 과정을 생략하고 바로 render 함수를 호출한다.) --> render(컴포넌트를 리렌더링 한다.) --> getSnapshotBeforeUpdate(컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드이다.) --> componentDidUpdate(컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드이다.) --> componentDidUpdate(컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드이다.)

## 언마운트(Unmount)
- 마운트의 반대 과정. 컴포넌트를 DOM에서 제거하는 것을 언마운트라고 한다.
- componentWillUnmount(컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드이다.)

## render() 함수
- 라이프사이클 메서드 중 유일한 필수 메서드❗️
- 이 메서드 안에서 this.props와 this.state에 접근 할 수 있으며 리액트 요소를 반환한다. 아무것도 보여주기 싫으면 null값이나 false를 반환하면 된다.
- DOM 정보를 가져오거나 state 변화를 줄 때는 componentDidMount에서 처리해야 한다.

##  constructor
- 컴포넌트를 만들 때 처음으로 실행된다. 이 메서드에서 state의 초기값을 정할 수 있다.

## getDerivedStateFromProps 메서드
- 리액트 버전 16.3 이후에 나온 메서드이다. props로 받아 온 값을 state에 동기화 시키는 용도로 사용한다. 컴포넌트가 마운트 될 때와 업데이트 될 때 호출한다.
```javascript
static getDerivedStateFromProps(nextProps, prevState) {
  if(nextProps.value !== prevState.value) { // 조건에 따라 특정 값 동기화
    return { value: nextProps.value };
  }
  return null // state를 변경할 필요가 없다면 null을 반환
}
```

## conponentDidMount 메서드
- 첫 렌더링을 다 마친 후 실행된다. 여기 안에서 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 하면 된다.

## shouldComponentUpdate 메서드
- props, state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드. 반드시 true or false 값을 반환해야 한다.
- 컴포넌트를 만들 때 이 메서드를 따로 생성하지 않으면 기본적으로 항상 true 값을 반환한다.
- false 값을 반환하게 되면 업데이트 과정은 여기서 중지된다.
- 메서드 안에서는 현재 props와 state는 this.props와 this.state로 접근하고, 새로 설정될 props, state는 nextProps와 nextState로 접근 할 수 있다. 
- 최적화를 적용할 때, 상황에 맞는 알고리즘을 작성해 리렌더링을 방지할 때는 false 값을 반환한다.

## componentDidUpdate 메서드
- 리렌더링을 완료한 후 실행한다. 업데이트가 끝난 직후이므로, DOM 관련 처리를 해도 무방하다.
- prevProps, prevState를 사용하여 컴포너트가 업데이트 전에 가졌던 데이터에 접근 할 수 있다.
- getSnapshotBeforeUpdate에서 반환한 값이 있다면 여기서 snapshot 값을 전달 받을 수 있다.

## componentWillUnmount 메서드
- 컴포넌트를 DOM에서 제거할 때 실행된다. componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 한다.

## componentDidCatch 메서드
- 리액트 버전 16 이후로 나온 메서드. 렌더링 도중에 에러가 발생했을 때 앱이 먹통이 되지 않고 오류 UI를 보여줄 수 있게 한다.
- error는 파라미터에 어떤 에러가 발생했는지 알려 주며, info 파라미터는 어디에 있는 코드에서 오류가 발생했는지에 대한 정보를 준다.
```javascript
componentDidCatch(error, info) {
  this.setState({
    error: true
  });
  console.log({error, info});
}
```

## 실습코드
- 라이크사이클 메서드를 실행할 때마다 콘솔에 기록하고 부모 컴포넌트에서 props로 색상을 받아 버튼을 누르면 state.number 값을 1씩 더한다.
- getDerivedStateFromProps는 부모에게서 받은 color 값을 state에 동기화 하고 있다.
- getSnapshotBeforeUpdate는 DOM에 변화가 일어나기 직전의 색상 속성을 snapshot 값으로 반환하여 이것을 comoponentDidUpdate에서 조회 할 수 있게 했다.
- 추가로 shouldComponentUpdate 메서드에서 state.number 값의 마지막 자리수가 4이면(4, 14, 24, 34...) 리렌더링을 취소하도록 했다.
- App.js에서 getRandomColor 함수는 state의 color 값을 랜덤 색상으로 설정한다. 16777214를 hex로 표현하면 ffffff가 된다. 이 코드는 000000부터 ffffff까지 표현 할 수 있다.
- ErrorBoundary.js 에서 componentDidCatch 메서드에서 this.state.error 값을 true로 업데이트 해준다. 그렇게 하고나면 this.state.error 값이 true 일 때 에러가 발생했을 때의 문구를 보여주게 된다.
```javascript
// App.js 부모 컴포넌트
import { Component } from "react";
import ErrorBoundary from "./ErrorBoundary";
import LifeCycleSample from "./LifeCycleSample";

// 랜덤 색상을 생성합니다.
function getRandomColor() {
  return "#" + Math.floor(Math.random() * 16777215).toString(16);
}

class App extends Component {
  state = {
    color: `#000000`,
  };

  handleClick = () => {
    this.setState({
      color: getRandomColor(),
    });
  };
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>랜덤색상</button>
        <ErrorBoundary>
          <LifeCycleSample color={this.state.color} />
        </ErrorBoundary>
      </div>
    );
  }
}

export default App;
***
// LifeCycleSample.js 자식 컴포넌트
import { Component } from "react";

class LifeCycleSample extends Component {
  state = {
    number: 0,
    color: null,
  };

  myRef = null; // ref를 설정할 부분

  constructor(props) {
    super(props);
    console.log("constructor");
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    console.log("getDerivedStateFromProps");
    if (nextProps.color !== prevState.color) {
      return { color: nextProps.color };
    }
    return null;
  }

  componentDidMount() {
    console.log("componentDidMount");
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log("shouldComponentUpdate", nextProps, nextState);
    // 숫자의 마지막 자리가 4면 리렌더링하지 않는다.
    return nextState.number % 10 !== 4;
  }

  componentWillUnmount() {
    console.log("componentWillUnmount");
  }

  handleClick = () => {
    this.setState({
      number: this.state.number + 1,
    });
  };

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log("getSnapshotBeforeUpdate");
    if (prevProps.color !== this.props.color) {
      return this.myRef.style.color;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log("componentDidUpdate", prevProps, prevState);
    if (snapshot) {
      console.log("업데이트 되기 직전 색상", snapshot);
    }
  }

  render() {
    console.log("render");
    const style = {
      color: this.props.color,
    };

    return (
      <div>
        {/* {this.props.missing.value} */} // 에러발생 했을 때 
        <h1 style={style} ref={(ref) => (this.myRef = ref)}>
          {this.state.number}
        </h1>
        <p>color: {this.state.color}</p>
        <button onClick={this.handleClick}>더하기</button>
      </div>
    );
  }
}
export default LifeCycleSample;
***
//  ErrorBoundary.js 에러 발생하면 보여줄 컴포넌트
import { Component } from "react";

class ErrorBoundary extends Component {
  state = {
    error: false,
  };

  componentDidCatch(error, info) {
    this.setState({
      error: true,
    });
    console.log({ error, info });
  }

  render() {
    if (this.state.error) return <div>에러가 발생했습니다.</div>;
    return this.props.children;
  }
}
export default ErrorBoundary;

```
## 정리🧑‍💻
- ![LifeCycle](https://user-images.githubusercontent.com/101798682/181191219-c486d658-fd80-4f14-8f23-8bc4bbddfff7.jpeg)
- 업데이트의 성능을 개선할 때는 shouldComponentUpdate가 중요하게 사용된다.
- 음.. 라이프 사이클은 클래스형에서 보다 굉장히 복잡하다는 느낌을 받았다. 이 전에 배울 때에는 클래스형보다는 함수형의 중점을 두고 공부를 했었고 클래스형에서는 생명주기를 이런식으로 활용한다 라는 걸 배웠었는데 책에선 보다 더 많은 기능들을 알려주고 있었다. 처음접하다 보니 좀 복잡한 감이 없지 않아 있지만 위에 예제 코드를 다시 뜯어보면서 코드를 다시 한 번 보고 이해하는 시간을 가져야겠다!



