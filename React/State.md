# State

- 리액트에서 State는 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다. props는 부모 컴포넌트에서 값을 설정해 자식요소로 넘겨주는 값이다. 값을 바꿀려면 부모 컴포넌트에서 설정을 다시 해주어야 한다.
- 두 가지 종류의 state가 있는데 하나는 클래스형 컴포넌트가 지니고 있는 state이고, 다른 하나는 함수 컴포넌트에서 useState라는 함수를 통해 사용하는 state이다.

## 클래스형 state

  ```javascript
    import { Component } from "react";
    class Counter extends Component {
    // 컴포넌트의 생성자 메서드
      constructor(props) {
      // state의 초깃값 설정하기
      super(props);
      this.state = {
        number: 0,
        fixedNumber: 0,
      };
    }

    render() {
      const { number } = this.state; // state를 조회할 때는 this.state로 조회한다.
    return (
      <div>
        <h1>{number}</h1>

        <button
          // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
          onClick={() => {
            // this.setState를 사용하여 state에 새로운 값을 넣을 수 있습니다.
            this.setState({ number: number + 1 });
          }}
        >
          +1
        </button>
      </div>
    );
    }
  }
    export default Counter;
  ```

- state 안에 여러 값이 있을 때
  ```javascript
    import { Component } from "react";
    class Counter extends Component {
      // 컴포넌트의 생성자 메서드
      constructor(props) {
      // state의 초깃값 설정하기
      super(props);
      this.state = {
        number: 0,
        fixedNumber: 0,
      };
    }

    render() {
      const { number, fixedNumber } = this.state; // state를 조회할 때는 this.state로 조회한다.
    return (
      <div>
        <h1>{number}</h1>
        <h2>바뀌지 않는 값: {fixedNumber}</h2>
        <button
          // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
          onClick={() => {
            // this.setState를 사용하여 state에 새로운 값을 넣을 수 있습니다.
            this.setState({ number: number + 1 });
          }}
        >
          +1
        </button>
      </div>
    );
    }
  }
  export default Counter;
  ```
- constructor 메서드를 선언하지 않고도 state 초기값을 설정할 수 있다.
  ```javascript
    class Counter extends Component {
      state = {
        number: 0,
        fixedNumber: 0
      };
      (...)
   ```
-  state 값을 업데이트 할때는 상태가 비동기적으로 업데이트 된다. (두 번 사용해도 state값은 1씩 증가한다.)
-  앞에서 prevState는 기존 상태이고, props는 현재 지니고 있는 것을 뜻한다. 업데이트 과정에서 props가 필요하지 않다면 생략 가능하다.
-  화살표 함수에서 값을 바로 반환하고 싶다면 { }를 생략하면 된다. [더 자세한 내용](https://github.com/hailjeong/TIL/blob/main/React/Component.md)여기 맨 밑 부분에 작성해놨다.
   ```javascript
      <button
        // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
        onClick={() => {
          this.setState((prevState) => {
            return {
              number: prevState.number + 1,
            };
          });
          // 위에 코드와 아래 코드는 완전히 똑같은 기능을 한다.
          // 아래 코드는 함수에서 바로 객체를 반환한다는 뜻이다. 알아두자
          this.setState(prevState => ({
            number: prevState.number + 1
          }))
        }}
      >
        +1
      </button>
   ```
   
## 함수형 Component state
- 리액트 버전 16.8 이전에는 state를 선언할 수 없었지만 16.8이후부터 useState라는 것을 사용해 함수형 컴포넌트에서도 state를 사용할 수 있게 되었다.
- 이러한 과정에서 Hooks를 사용하게 되는데 이건 다음에 더 자세히 알아볼 수 있도록 하자.
- 배열 비구조화 할당
  - 배열 비구조화 할당은 객체 비구조화 할당과 비슷하다. 배열 안에 들어 있는 값을 쉽게 추출할 수 있도록 해주는 문법이다.
  ```javascript                               
  const array = [1, 2];                      
  const one = array[0];  
  const twto = array[1];
  
  //  배열 비구조화 할당을 사용하면 아래처럼 바뀐다.
  
  const array = [1, 2];
  const [one, two] = array;
  ```
- useState 사용하기
  - useState 함수의 인자에는 상태의 초기값을 넣어 준다.
  - useState 에서는 반드시 객체가 아니여도 상관 없다. 숫자, 문자, 배열
  - 함수를 호출하면 배열이 반환된다. 첫번째 원소는 현재 상태이고, 두번째 원소는 상태를 바꾸어주는 함수이다. 이것을 세터(setter)함수라고 한다.
```javascript 
  import { useState } from "react";

  const Say = () => {
    const [message, setMessage] = useState("");
    const onClickEnter = () => setMessage("안녕하세요");
    const onClickLeave = () => setMessage("안녕히 계세요");

  return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1>{message}</h1>
    </div>
  );
};

export default Say;
```
  - state를 여러번 사용 할 수도 있다.
```javascript
  import { useState } from "react";

  const Say = () => {
    const [message, setMessage] = useState("");
    const [color, setColor] = useState("black");
    const onClickEnter = () => setMessage("안녕하세요");
    const onClickLeave = () => setMessage("안녕히 계세요");

  return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1 style={{ color }}>{message}</h1>
      <button style={{ color: "red" }} onClick={() => setColor("red")}>
        빨간색
      </button>
      <button style={{ color: "green" }} onClick={() => setColor("green")}>
        초록색
      </button>
      <button style={{ color: "blue" }} onClick={() => setColor("blue")}>
        파란색
      </button>
    </div>
  );
};

export default Say;
```
  - 주의해야 할 점
    - state값을 바꿀 때는 setState혹은 useState를 통해 전달받은 세터 함수를 사용해야 한다.
    - 배열이나 객체 사본을 만든 후, 그 사본에 값을 업데이트 한 후에 그 사본의 상태를 setState혹은 세터함수를 통해 업데이트 한다.

- 정리🧑‍💻
  - props는 부모 컴포넌트가 설정하고, state는 컴포넌트 자체적으로 지닌 값으로 컴포넌트 내부에서 값을 업데이트 할 수 있다.
  - state는 앞으로 대부분 useState를 사용하게 된다. 즉 함수형 컴포넌트를 사용하게 될 것이다. 이유는 코드도 간결해지고, 리액트 자체적으로 함수 컴포넌트와 사용할 수 있는 hooks 사용하는 것이 주요 컴포넌트 개발 방식이다. 
  
  
  
  
