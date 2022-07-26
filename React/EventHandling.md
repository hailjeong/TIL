# Event Handling(이벤트 핸들링)

- 사용자가 웹 브라우저에서 DOM(Document Object Model)요소들과 상호 작용하는 것을 이벤트라고 한다.
- 마우스를 올릴 땐 onmouseover, 클릭했을 땐 onclick, Form 요소의 값이 바뀔 때는 onchange 이벤트를 실행한다.
- 이벤트를 사용할 때 주의 사항
  - 이벤트 이름은 카멜표기법으로 작성한다. 예) onClick, onKeyUp
  - 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 객체를 전달합니다. 
  - DOM요소에만 이벤트를 설정할 수 있다(div, button, form, span등등). 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없다.
  - 전달받은 props를 컴포넌트 내부의 DOM 이벤트로 설정할 수는 있다.

- 리액트에서 지원하는 이벤트 종류
  - Clipboard, Touch, Composition, UI, Keyboard...... 이벤트가 굉장히 많다.
  - [더 많은 정보](https://facebook.github.io/react/docs/events.html)을 참고하면 된다.

- onChange 이벤트 생성
```javascript
  import { Component } from "react";

  class EvetnPractice extends Component {
    render() {
      return (
        <div>
          <h1>이벤트 연습</h1>
          <input
            type="text"
            name="message"
            placeholder="아무거나 입력하세요"
            onChange={(e) => {
              console.log(e);
            }}
          />
        </div>
      );
    }
  }
export default EvetnPractice;
```
  - 여기서 콘솔에 찍히는 e는 SyntheticEvent로 이벤트를 감싸는 객체이다. 이벤트가 끝나고 나면 이벤트가 초기화 되서 정보를 참조 할 수가 없다.
  - 비동기적으로 이벤트 객체를 참조할 일이 있다면 e.persist()함수를 호출해 주어야 한다. 
  - console.log(e.target.value)를 찍게 되면 인풋 값이 바뀔 때 마다 콘솔에 값이 찍힌다.

- state에 input값 담기
  - 이 코드를 이용하면 인풋값에 값을 넣고 확인을 클릭했을 때 인풋값에 들어있는게 alert알림창으로 뜬다. 
```javascript
  import { Component } from "react";

  class EvetnPractice extends Component {
    state = {
      message: "",
    };
    render() {
      return (
        <div>
          <h1>이벤트 연습</h1>
          <input
            type="text"
            name="message"
            placeholder="아무거나 입력하세요"
            value={this.state.message}
            onChange={(e) => {
              this.setState({ message: e.target.value });
            }}
          />
          <button
            onClick={() => {
              alert(this.state.message);
              this.setState({
                message: "",
              });
            }}
          >
            확인
          </button>
        </div>
      );
    }
  }
export default EvetnPractice;
```
- 임의 메서드 만들기
  - 이렇게 하는 이유는 가독성을 좋게 하기 위함이다.
  - 기본 방식
  ```javascript
    import { Component } from "react";

    class EvetnPractice extends Component {
      state = {
        message: "",
      };
      constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleClick = this.handleClick.bind(this);
      }
      handleChange(e) {
        this.setState({
          message: e.target.value,
        });
      }
      handleClick() {
        alert(this.state.message);
        this.setState({
          message: "",
        });
      }
      render() {
        return (
          <div>
            <h1>이벤트 연습</h1>
            <input
              type="text"
              name="message"
              placeholder="아무거나 입력하세요"
              value={this.state.message}
              onChange={this.handleChange}
            />
            <button onClick={this.handleClick}>확인</button>
          </div>
        );
      }
    }

    export default EvetnPractice;
  ```
  - Property Initializer Syntax를 사용한 방법
    - 메서드 바인딩은 생성자 메서드에서 하는 것이 정석이다.
    - 새 메서드를 만들 때마다 constructor도 수정해주야 하기 때문에 불편하다고 느낄 수 있다.
    - 이 방법을 사용하면 조금 더 간단하게 할 수 있다.
    - 위 코드와 비교 했을 때 이 부분만 달라졌다.
    ```javascript
      handleChange = (e) => {
        this.setState({
          message: e.target.value,
        });
      };

      handleClick = () => {
        alert(this.state.message);
        this.setState({
          message: "",
        });
      };
    ```
- 여러개의 input값
  - e.target.name을 활용하는 방법. e.target.name은 해당 인풋의 name을 가리킨다. 
```javascript
  import { Component } from "react";

  class EvetnPractice extends Component {
    state = {
      username: "",
      message: "",
    };

    handleChange = (e) => {
      this.setState({
        [e.target.name]: e.target.value,
      });
    };

    handleClick = () => {
      alert(this.state.username + ": " + this.state.message);
      this.setState({
        message: "",
      });
    };

    render() {
      return (
        <div>
          <h1>이벤트 연습</h1>
          <input
            type="text"
            name="username"
            placeholder="사용자명"
            value={this.state.username}
            onChange={this.handleChange}
          />
          <input
            type="text"
            name="message"
            placeholder="아무거나 입력하세요"
            value={this.state.message}
            onChange={this.handleChange}
          />
          <button onClick={this.handleClick}>확인</button>
        </div>
      );
    }
  }
export default EvetnPractice;
// 결과 => '사용자명': "message"
```
- onKeyPress이벤트
  - 키를 눌렀을 때 발생하는 keyPress이벤트
  - 위 코드에서 handleKeyPress 이벤트를 추가해주고 두번째 input안에 이벤트를 추가해주면 된다.
  - 이렇게 하면 두번째 인풋에서 텍스트를 입력하고 Enter를 키를 누르게 되면 handleClick이벤트가 실행된다.
  ```javascript
    handleKeyPress = (e) => {
      if (e.key === "Enter") {
        this.handleClick();
      }
    };
    
    <input
      type="text"
      name="message"
      placeholder="아무거나 입력하세요"
      value={this.state.message}
      onChange={this.handleChange}
      onKeyPress={this.handleKeyPress}
    />
  ```
- 함수형 컴포넌트 
  - 위에 코드에서는 함수 두개를 따로 만들어주었고, 아래의 코드는 e.target.name을 활용했다. 
  - input값들이 많아지면 아래 코드처럼 form 객체를 사용해주는것도 좋을 것 같다. 
```javascript
import { useState } from "react";

const EventPracticeFunc = () => {
  const [username, setUsername] = useState("");
  const [message, setMessage] = useState("");
  const onChangeUsername = (e) => {
    setUsername(e.target.value);
  };
  const onChangeMessage = (e) => {
    setMessage(e.target.value);
  };
  const onClick = () => {
    alert(username + ": " + message);
    setUsername();
    setMessage();
  };
  const onKeyPress = (e) => {
    if (e.key === "Enter") {
      onClick();
    }
  };
  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type="text"
        name="username"
        placeholder="사용자명"
        value={username}
        onChange={onChangeUsername}
      />
      <input
        type="text"
        name="message"
        placeholder="메시지입력란"
        value={message}
        onChange={onChangeMessage}
        onKeyPress={onKeyPress}
      />
      <button onClick={onClick}>확인</button>
    </div>
  );
};
export default EventPracticeFunc;
***

import { useState } from "react";

const EventPracticeFunc = () => {
  const [form, setForm] = useState({
    username: "",
    message: "",
  });
  const { username, message } = form;

  const onChange = (e) => {
    const nextForm = {
      ...form, // 기존의 form 내용을 이 자리에 복사한 뒤
      [e.target.name]: e.target.value, // 원하는 값을 덮어 씌우기
    };
    setForm(nextForm);
  };

  const onClick = () => {
    alert(username + ": " + message);
    setForm({
      username: "",
      message: "",
    });
  };
  const onKeyPress = (e) => {
    if (e.key === "Enter") {
      onClick();
    }
  };
  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type="text"
        name="username"
        placeholder="사용자명"
        value={username}
        onChange={onChange}
      />
      <input
        type="text"
        name="message"
        placeholder="메시지입력란"
        value={message}
        onChange={onChange}
        onKeyPress={onKeyPress}
      />
      <button onClick={onClick}>확인</button>
    </div>
  );
};
export default EventPracticeFunc;

```

- 정리
  - 여러개의 인풋 값을 관리하기 위해 useState에서 form 객체를 사용했다. 잘 기억해두자! 
  - 리액트의 장점 중 하나는 자바스크립트에 익숙하다면 쉽게 활용 할 수 있다는 점이다. 자바스크립트에 대한 공부도 꾸준하게 해주는게 좋을것 같다.
