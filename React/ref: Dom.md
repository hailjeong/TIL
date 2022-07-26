# ref: Dom에 이름달기
- 일반 HTML에서 DOM 요소에 이름을 달 때는 id를 사용한다.
- id를 달면 CSS 작업할 때 특정 id에 특정 스타일을 지정할 수 있고, 자바스크립트에서 해당 id를 가진 요소르 찾아 작업을 할 수 있다.
- 이러한 것처럼 리액트에서도 DOM에 이름을 다는 방법이 있다. 그것은 ref(reference)를 활용하는 것이다.
- 리액트 컴포넌트 안에서도 id를 활용할 수는 있다. 하지만 권장하지 않는다. 그 이유는 컴포넌트를 여러 번 사용하게 될 때, 아이디 값은 유일해야 되는데 여러개의 중복된 id가 생기기 때문이다. 이때 ref를 사용하게 되면 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하기 때문에 이런 문제가 생기지 않는다.
- 라이브러리나 프레임워크와 함께 id를 사용해야 하는 상황이 발생할 때에는 id 뒷부분에 추가 텍스트를 붙여서 중복 id가 발생하는 것을 방지해야 한다.

## ref는 언제 사용?
- 특정 DOM에 작업을 할 때 ref를 사용한다. 그렇다면 여기서 말하는 작업이란 무엇일까..?
- 여기서 말하는 작업은 "DOM을 꼭! 직접적!으로 건드려야 할 때" 이다.

## 클래스형 컴포넌트
- 먼저 css파일을 불러와서 input의 className값의 변화에 따라 색깔이 바뀌게 된다.
- '0000'이면 success를 반환하고 다른값이 들어가면 failure이 들어가면서 색깔이 바뀐다.

```javascript
// css 파일
.success {
  background-color: lightgreen;
}
.failure {
  background-color: lightcoral;
}
***
import { Component } from "react";
import "./ValidationSample.css";

class ValidationSample extends Component {
  state = {
    password: "",
    clicked: false,
    validated: false,
  };
  handleChange = (e) => {
    this.setState({
      password: e.target.value,
    });
  };
  handleButtonClick = () => {
    this.setState({
      clicked: true,
      validated: this.state.password === "0000",
    });
  };
  render() {
    return (
      <div>
        <input
          type="password"
          value={this.state.password}
          onChange={this.handleChange}
          className={
            this.state.clicked
              ? this.state.validated
                ? "success"
                : "failure"
              : ""
          }
        />
        <button onClick={this.handleButtonClick}>확인하기</button>
      </div>
    );
  }
}
export default ValidationSample;
```

# DOM을 꼭 사용해야 하는 상황
- 아래 상황에서는 DOM에 직접적으로 접근해야 해서 ref를 사용한다.
  - 특정 input에 포커스 주기
  - 스크롤 박스 조작하기
  - Canvas요소에 그림 그리기 등

## ref 사용
- 가장 기본적인 방법은 콜백 함수를 사용하는 것이다.
- ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달해 주면 된다.
- this.input은 input 요소의 DOM을 가리킨다.
- ref의 이름은 원하는 것으로 자유롭게 지정할 수 있다. DOM 타입과 관계없이 this.superman = ref 이렇게 해도 상관없다.
- 예) ```javascript
      <input ref={(ref) => {this.input=ref}} />
      ```
- createRef를 통한 설정
  - 더 적은 코드로 쉽게 사용할 수 있다.
  - 리액트 버전 16.3 이후에 나왔다.
  - 컴포넌트 내부에서 멤버 변수로 React.createRef()를 담아주어야 한다.
  - 해당 멤버 변수를 ref를 달고자 하는 요소에 ref props로 넣어주면 된다.
  - 나중에 DOM에 접근하려면 this.input.current를 조회하면 된다.
  - 콜백 함수를 사용할 때와 다른 점은 뒷부분에 .current를 넣어주어야 하는 점이다.

```javascript
import React, { Component } from "react";

class CreateRef extends Component {
  input = React.createRef();

  handleFocus = () => {
    this.input.current.focus();
  };

  render() {
    return (
      <div>
        <input ref={this.input} />
      </div>
    );
  }
}
export default CreateRef;
```
## 컴포넌트에 ref달기
- 리액트에서는 컴포넌트에도 ref를 달 수 있다.
- 주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓴다.
- 아래 코드처럼 쓰면 MyComponent 내부의 메서드 및 멤버 변수에도 접근 할 수 있다. 즉 내부의 ref에도 접근 할 수 있다.
```javascript
<MyComponent ref={(ref) => {this.myComponent = ref}}/>
```
- 스크롤 바를 맨 아래쪽을 내리는 메서드 만들기
- 자바스크립트로 스크롤바를 내릴 때 DOM 노드가 가진 다음 값들을 사용한다.
  - scrollTop: 세로 스크롤바 위치
  - scrollHeight: 스크롤이 있는 박스 안의 div 높이
  - clientHeight: 스크롤이 있는 박스의 높이
- 스크롤바를 맨 아래쪽으로 내리려면 scrollHeight에서 clientHeight를 빼면된다.
- 주의 할 점
  - 문법상으로는 onClick = {this.scrollBox.scrollToBottom} 같은 형식으로 작성해도 틀린 것은 아니다.
  - 다만 처음에 렌더링 될 때 this.scrollBox값이 undefined이므로 this.scrollBox.scrollToBottom 값을 읽어 오는 과정에서 오류가 발생한다.
  - 화살표 함수를 사용하여 새로운 함수를 만들고 그 내부에서 this.scrollBox.scrollToBottom 메서드를 실행하면, 버튼을 누를 때 this.scrollBox.scrollToBottom 값을 읽어 와서 실행하므로 오류가 발생하지 않는다. 
```javascript
// ScrollBox.js
import { Component } from "react";

class ScrollBox extends Component {
  scrollToBotton = () => {
    const { scrollHeight, clientHeight } = this.box;
    //  앞 코드에는 비구조화 할당 문법을 사용했다.
    //  다음 코드와 같은 의미이다.
    //  const scrollHeight = this.box.scrollHeight;
    //  const clientHeight = this.box.clientHeight;
    this.box.scrollTop = scrollHeight - clientHeight;
  };
  
  scrollToTopp = () => {
    this.box.scrollTop = 0;
  };

  render() {
    const style = {
      border: "1px solid black",
      height: "300px",
      width: "300px",
      overflow: "auto",
      position: "relative",
    };
    const innerStyle = {
      width: "100%",
      height: "650px",
      background: "linear-gradient(white,black)",
    };
    return (
      <div style={style} ref={(ref) => (this.box = ref)}>
        <div style={innerStyle} />
      </div>
    );
  }
}
export default ScrollBox;

// App.js
import { Component } from "react";
import ScrollBox from "./ScrollBox";

class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={(ref) => (this.scrollBox = ref)} />
        <button onClick={() => this.scrollBox.scrollToBotton()}>
          맨밑으로
        </button>
        <button onClick={() => this.scrollBox.scrollToTopp()}>맨 위로</button>
      </div>
    );
  }
}

export default App;
```

## 함수형 컴포넌트
- 함수형 컴포넌트에서 ref를 사용하려면 Hooks를 사용해야 히기 때문에 좀 나중에 Hooks를 배우면서 ref사용법을 배워보도록 하자.
- useRef라는 Hook함수를 사용한다. 자세한 내용은 추후에 배우게 된다.

## 정리
- 배운걸 작성하는데 의미를 두지 말고 작성을 잘 해놓고 계속 꾸준하게 볼 수 있는 내가 되어 누군가에게 설명을 잘해줄 수 있을 정도까지의 사람이 되자!!
- 컴포넌트 내부에서 DOM에 직접 접근해야 할 때는 ref를 사용한다.
- 만약 ref를 사용하지 않고 원하는 기능을 구현할 수 있으면 사용하지 않아도 된다. 처음에는 state로만 구현하였다.
- 서로 다른 컴포넌트끼리 데이터를 교류할 때 ref를 사용한다면 이는 잘못 이해한거다. 물론 할 수 있지만 이렇게 사용하게 되면 앱 규모가 커지면 꼬이게 되어 유지 보수가 힘들어진다.
- 컴포넌트끼리 데이터를 교류할 때는 언제나 데이터를 부모->자식 이 흐름을 까먹지 말자!




