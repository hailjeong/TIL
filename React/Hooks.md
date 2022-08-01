# Hooks
- 리액트 버전 16.8 이후에 새로 추가되었다. 함수형 컴포넌트에서도 상태 관리를 할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공한다.

## useState
- 먼저 useState(0) 기본값을 설정해준다. 
```javascript
// App.js 부모 컴포넌트
import Counter from "./Counter";

const App = () => {
  return (
    <div>
      <Counter />
    </div>
  );
};

export default App;

// Counter.js 자식 컴포넌트
import { useState } from "react";

const Counter = () => {
  const [value, setValue] = useState(0);

  return (
    <div>
      <h1>
        현재 카운터 값은 <b>{value}</b>입니다.
      </h1>
      <button onClick={() => setValue(value + 1)}> +1</button>
      <button onClick={() => setValue(value - 1)}> -1</button>
    </div>
  );
};
export default Counter;
```

## useState 여러 번 사용하기
- 여러개의 useState를 여러 번 사용하면 된다.

```javascript
// App.js 부모 컴포넌트
import Info from './Info';

const App = () => {
  return <Info />;
};

export default App;

// Info.js 자식 컴포넌트
import { useEffect, useState } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nickname, setNickName] = useState("");
  
  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNickName = (e) => {
    setNickName(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickName} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};
export default Info;
```

## useEffect
- 클래스형에서도 우리는 이미 한 적이 있다.
- 이 코드를 실행시켜 브라우저에서 개발자 도구를 열고 인풋의 내용을 변경할 때 콘솔에 {name:"", nickname:""} 여기에 변화가 생긴다.
```javascript
import { useEffect, useState } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nickname, setNickName] = useState("");

  useEffect(() => {
    console.log("렌더링 완료");
    console.log({
      name,
      nickname,
    });
  });

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNickName = (e) => {
    setNickName(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickName} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};
export default Info;
```
- 마운트 될 때만 실행하고 싶을 때
  - 두 번째 파라미터로 비어 있는 배열을 넣어주면 된다.
  - 위에 있는 코드 중에 useEffetc를 이렇게 바꿔주면 된다.
  - 컴포넌트가 처음 나타날 때만 콘솔에 문구가 나타나고 그 이후에는 나타나지 않는다.
  ```javascript
  useEffect(() => {
    console.log("마운트 될 때만 실행된다.")
  }, []);
  ```
- 특정 값이 업데이트될 때만 실행하고 싶을 때
  - 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어 주면 된다.
  - 아래코드로 수정해줄 경우 name인풋 값이 업데이트 될 때만 콘솔에 찍히고 nickname의 인풋값이 바뀔 때는 콘솔에 아무것도 찍히지 않는다.
  ```javascript
  useEffect(() => {
    console.log(name);
  }, [name]);
  ```
- 뒷정리하기
  - useEffect는 기본적으로 렌더링 되고 난 직후마다 실행되며, 두 번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라지게 된다. 
  - 컴포넌트가 언마운트 되기 전이나 업데이트 되기 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리 함수를 반환해주어야 한다.
  - 아래 코드를 실행할 경우 보이기/숨기기 버튼이 있을 것이다.
  - 보이기를 눌러 컴포넌트가 나타날 때에는 콘솔에 effect 가 찍힐 것이고, 사라질 때는 cleanup이 찍힐 것이다. 
  - name의 인풋값이 바뀔 때마다 콘솔에 변화가 생기는 것을 볼 수 있다.
  - 렌더링 될 때마다 뒷정리 함수가 계속 나타나는 것을 확인할 수 있는데 함수가 호출될 때는 업데이트 되기 직전의 값을 보여주는 것도 확인할 수 있다.
  ```javascript
  // App.js 부모 컴포넌트
  import { useState } from "react";
  import Info from "./Info";

  const App = () => {
    const [visible, setVisible] = useState(false);
    return (
      <div>
        <button
          onClick={() => {
            setVisible(!visible);
          }}
        >
          {visible ? "숨기기" : "보이기"}
        </button>
        <hr />
        {visible && <Info />}
      </div>
    );
  };

  export default App;

  // Info.js 자식 컴포넌트
  import { useEffect, useState } from "react";

  const Info = () => {
  const [name, setName] = useState("");
  const [nickname, setNickName] = useState("");

  useEffect(() => {
    console.log("effect");
    console.log(name);
    return () => {
      console.log("cleanup");
      console.log(name);
    };
  }, [name]);

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNickName = (e) => {
    setNickName(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickName} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
  };
  export default Info;
  ```
  
## useReducer
- useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트 해주고싶을 때 사용하는 Hook이다.
- 나중에 리듀서(reducer)라는 개념을 배우면서 한 번 더 체크하면 좋을 것 같다.
- 리듀서는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action)값을 전달받아 새로운 상태를 반환하는 함수이다. 새로운 상태를 만들 때는 반드시 불변성을 지켜주어야 한다.
- 액션값은 주로 이런 형태로 이루어져 있다. 나중에 배울 리덕스에서는 사용하는 액션 객체에 어떤 액션인지 알려 주는 type 필드가 꼭 있어야 하지만, useReducer에서 사용하는 액션 객체에는 반드시 type을 지니고 있을 필요가 없다. 객체가 아니라 문자열이나 숫자여도 상관이 없다.
```javascript
{
  type: 'INCREMENT'
  // 다른 값들이 필요하다면 추가로 들어간다.
}
```
- useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣어준다. 
- 이 Hook을 사용하면 state 값과 dispatch함수를 받아 오는데 여기서 state는 현재 가리키고 있는 상태고, dispatch는 액션을 발생시키는 함수이다.
- dispatch(action)과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어 주면 리듀서 함수가 호출되는 구조이다.
- useReducer를 사용했을 때의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 것이다.
```javascript
import { useReducer } from "react";

function reducer(state, action) {
  // action.type에 따라 다른 작업 수행
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      // 아무것도 해당되지 않을 때 기존 상태 반환
      return state;
  }
}
const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });

  return (
    <div>
      <h1>
        현재 카운터 값은 <b>{state.value}</b>입니다.
      </h1>
      <button onClick={() => dispatch({ type: "INCREMENT" })}> +1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}> -1</button>
    </div>
  );
};
export default Counter;
```
## useReducer를 사용하여 Info 컴포넌트에서 인풋 상태를 관리하기
- 앞에서 배울때는 인풋이 여러 개 일 경우 useState를 여러 번 사용했는데 useReducer를 사용하면 기존에 클래스형 컴포넌트에서 input 태그에 name 값을 할당하고 e.target.name을 하여 setState를 해준 것과 비슷한 방식으로 작업을 처리할 수 있다.
- useReducer에서의 액션은 그 어떤 값도 사용 가능하다. 아래 코드에서는 이벤트 객체가 지니고 있는 e.target 값 자체를 액션 값으로 사용했다. 이런 식의 코드를 치면 갯수가 많아져도 코드의 가독성이 좋아진다.
```javascript
// Info.js 자식파일
import { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: "",
    nickname: "",
  });
  const { name, nickname } = state;

  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};
export default Info;
```

## useMemo
- useMemo를 사용하면 함수 컴포넌트 내부에서 발생하는 연산을 최적화 할 수 있다.
- 평균을 구하는 코드
- 아래 코드를 실행해 콘솔 창을 열어 확인해보면 인풋값을 등록 할 때뿐만 아니라 인풋 내용이 수정될 때도 getAverage ㅎ마수가 호출되는 것을 확인 할 수 있다.("평균값 계산 중"이 계속 찍힌다.)
- 인풋값이 바뀔 때에는 계산할 필요가 없기 때문에 이 렌더링을 막아지구 위해 useMemo를 사용한다. 최적화를 위해❗️
- 렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식이다.
- 아래 코드에서 ```javascript const avg = useMemo(() => getAverage(list), [list]);``` 이 코드를 입력해주면 list 배열의 내용이 바뀔 때만 getAverage 함수가 호출된다.
```javascript
import { useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  // Enter키를 누르면 onInsert 함수가 실행되어 등록 된다.
  const onKeyPress = (e) => {
    if (e.key === "Enter") {
      onInsert();
    }
  };
  return (
    <div>
      <input value={number} onChange={onChange} onKeyPress={onKeyPress} />
      <button onClick={onInsert}>등록 </button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {getAverage(list)}
      </div>
    </div>
  );
};
export default Average;
```
## useCallback
- useCallback은 useMemo와 비슷한 함수이다.
- 주로 렌더링 성능을 최적화해야 할 때 사용한다.
- 이 Hook을 사용하면 만들어 놨던 함수를 재사용할 수 있다.
- 컴포넌트의 렌더링이 자주 발생하거나 렌더링해야 할 컴포넌트의 개수가 많아지면 이 부분을 최적화 해 주는 것이 좋다.
- useCallback의 첫 번째 파라미터에는 생성하고 싶은 함수를 넣고, 두 번째 파라미터에는 배열을 넣으면 된다.
- onChange처럼 비어 있는 배열을 넣게 되면 컴포넌트가 렌더링될 때 만들었던 함수를 계속해서 재사용하게 되며 onInsert처럼 배열 안에 number와 list를 넣게 되면 인풋 내용이 바뀌거나 새로운 항목이 추가될 때 새로 만들어진 함수를 사용하게 된다.
- 함수 내부에서 상태 값에 의존해야 할 때는 그 값을 반드시 두 번째 파라미터 안에 포함시켜 주어야 한다. onInsert는 기존의 number와 list를 조회해서 nextList를 생성하기 때문에 배열 안에 number와 list를 꼭 넣어주어야 한다.
```javascript
import { useCallback, useMemo, useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성

  const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  }, [number, list]); // number, list가 바뀌었을 때만 함수 생성

  const onKeyPress = (e) => {
    if (e.key === "Enter") {
      onInsert();
    }
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} onKeyPress={onKeyPress} />
      <button onClick={onInsert}>등록 </button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};
export default Average;
```

## useRef
- 이 전에 ref를 배우면서 클래스형에서도 잠깐 다루어 봤지만 여기에서는 함수형 컴포넌트 안에서의 useRef Hook을 사용해 ref를 쉽게 사용할 수 있게 해준다.
```javascript
import { useCallback, useMemo, useRef, useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");
  const inputEl = useRef(null);

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성

  const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
    inputEl.current.focus();
  }, [number, list]); // number, list가 바뀌었을 때만 함수 생성

  const onKeyPress = (e) => {
    if (e.key === "Enter") {
      onInsert();
    }
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input
        value={number}
        onChange={onChange}
        ref={inputEl}
        onKeyPress={onKeyPress}
      />
      <button onClick={onInsert}>등록 </button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};
export default Average;
```
- 로컬 변수 사용하기
  - 로컬변수란 렌더링과 상관없이 바뀔 수 있는 값을 의미한다.
  ```javascript
    import {useRef} from 'react';
    const RefSample = () => {
      const id = useRef(1)
      const setId = (n) => {
        id.current = n;
      }
      const printId = () => {
        console.log(id.current);
      }
      return (
        <div>
          refsample
        </div>
      )
      }
      export default RefSample;
    ```

## 커스텀 Hooks 만들기
- 여러 컴포넌트에서 비슷한 기능을 공유할 경우, 이를 나만의 Hook으로 작성하여 로직을 재사용 할 수 있다.
- 기존 Info 컴포넌트에서 여러 개의 인풋을 관리하기 위해 useReducer로 작성했던 로직을 useInputs라는 Hook으로 따로 분리했다.
```javascript
// useInputs.js 컴포넌트
import { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

export default function useInputs(initialForm) {
  const [state, dispatch] = useReducer(reducer, initialForm);

  const onChange = (e) => {
    dispatch(e.target);
  };

  return [state, onChange];
}

//  Info.js 컴포넌트
import useInputs from "./useInputs";

const Info = () => {
  const [state, onChange] = useInputs({
    name: "",
    nickname: "",
  });
  const { name, nickname } = state;

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};
export default Info;
```

## 정리🧑‍💻
- 다른 개발자가 만든 Hooks도 라이브러리로 설치하여 사용할 수 있다. 더 많은 Hooks리스트 보기
  - [react-Hooks](https://nikgraf.github.io/react-hooks)
  - [awesome-react-Hooks](https://github.com/rehooks/awesome-react-hooks)
- 리액트 Hooks 패턴을 사용하면 굳이 클래스형 컴포넌트를 사용하지 않아도 된다. 대부분의 기능을 구현 할 수 있기 때문이다. 
- 리액트 메뉴얼에 따르면 기존의 클래스형 컴포넌트에 앞으로도 계속해서 지원을 할 예정이지만, 새로 작성하는 컴포넌트의 경우 함수형 컴포넌트와 Hook을 사용할 것을 권장한다.




















 





