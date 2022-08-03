# Todo 프로젝트
- react-icons 라이브러리를 사용했는데 다양하고 예쁜 아이콘을 사용할 수 있는 라이브러리이다. [사용방법](https://react-icons.netlify.com/) 아이콘의 크기나 색상은 props 혹은 css스타일로 변경할 수 있다.
- TodoTemplate: 화면을 가운데 정렬시켜, 앱 타이틀을 보여준다. children으로 내부 JSX를 props로 받아 와서 렌더링해 줍니다.
- TodoInsert: 새로운 항목을 입력하고 추가할 수 있는 컴포넌트. state를 통해 인풋 상태관리
- TodoKistItem: 각 할 일 항목에 대한 정보를 보여주는 컴포넌트. todo객체를 props로 받아 와서 상태에 따라 다른 스타일의 UI를 보여준다.
- TodoList: 배열을 props로 받아 온 후, map을 사용해서 여러 개의 TodoKistItem컴포넌트로 변환하여 보여준다.
- css에서 flex에 대해 이해가 가지 않을 때 [flex알아보기](htt://flexboxFroggy.com/#ko)
- [react-icons](https://react-icons.netlify.com/#/icons/md) 여기 들어가면 아이콘들을 많이 볼 수 있다.
- 기본적인 스타일링이 마친 뒤, 기능을 이제 넣어주기 시작한다. 
- 먼저 App.js에서 useState를 사용해 todos의 상태를 정의 한다.
- 이걸 TodoListItem에 넘겨주어 todos를 map으로 뿌려준다. map을 사용했기 때문에 key값이 있어야 하고 이걸 고유한 id 값으로 줬다.
- todo 데이터를 통째로 전달해주는것이 성능 최적화를 할 때 편리하다. 이유는 여러 종류의 값이 있을수도 있기 때문이다.
- TodoInsert value를 useState를 활용해서 상태관리를 한다. onChange 함수도 작성해야 하는데 이 과정에서 컴포넌트가 리렌더링될 때마다 함수를 새로 만드는 것이 아니라, 한 번 함수를 만들고 재사용할 수 있도록 useCallback을 사용했다.
- 일정을 추가하기 위해서는 새로운 함수를 하나 만들어줘야 한다. 새로운 객체를 만들때에도 id값이 있어야 하고 id값이 1씩 증가해야 중첩되는 값 없이 생성될 수 있다. 이때 useRef를 사용해 id값을 관리했다.
- props로 전달해야 할 함수를 만들 때 useCallback을 사용해 함수를 감싸는 것을 습관화 하면 좋다.
- onSubmit이라는 함수를 만들고 TodoInsert.js 컴포넌트에서 우리는 form의 onSubmit을 주었다. onSubmit은 브라우저를 새로고침 한다. 이를 방지하기 위해 e.preventDefault()를 선언해주어야 한다. 여기서 onClick 함수로도 충분히 구현할 수 있다. 하지만 onSubmit을 사용한 이유는 인풋에서 Enter버튼을 눌렀을 때도 발생하기 때문이다. onClick을 활용하면 onKeyPress를 한번 더 선언해줘야 해서 번거러워진다...
- 앞에서 배웠던것처럼 추가할 때에는 concat()을 활용하고 지우기 기능을 구현할때에는 filter를 사용하면 된다. 이유는 적용된 조건에 따라 그 조건에 true 값을 받아 새로운 배열을 만들기 때문이다.
- 지우기 기능을 구현 할 때, 함수에 자기자신이 가진 id를 넣어서 그 id값이 같으면 지운다 라는 조건을 사용해 하면 된다.
```javascript
// App.js 컴포넌트
import TodoTemplate from "./components/TodoTemplate";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import { useCallback, useRef, useState } from "react";

const App = () => {
  const [todos, setTodos] = useState([
    {
      id: 1,
      text: "팀플 마지막",
      checked: true,
    },
    {
      id: 2,
      text: "버그잡기",
      checked: true,
    },
    {
      id: 3,
      text: "배포하기",
      checked: false,
    },
  ]);

  // 고유값으로 사용될 id
  // ref를 사용하여 변수 담기
  const nextId = useRef(4);

  const onInsert = useCallback(
    (text) => {
      const todo = {
        id: nextId.current,
        text,
        checked: false,
      };
      setTodos(todos.concat(todo));
      nextId.current += 1; // nextId 1씩 더하기
    },
    [todos]
  );

  const onRemove = useCallback(
    (id) => {
      setTodos(todos.filter((todo) => todo.id !== id));
    },
    [todos]
  );

  const onToggle = useCallback(
    (id) => {
      setTodos(
        todos.map((todo) =>
          todo.id === id ? { ...todo, checked: !todo.checked } : todo
        )
      );
    },
    [todos]
  );

  return (
    <TodoTemplate>
      <TodoInsert onInsert={onInsert} />
      <TodoList todos={todos} onRemove={onRemove} onToggle={onToggle} />
    </TodoTemplate>
  );
};

export default App;

// TodoListItem.js 컴포넌트
import {
  MdCheckBoxOutlineBlank,
  MdRemoveCircleOutline,
  MdCheckBox,
} from "react-icons/md";
import cn from "classnames";
import "./TodoListItem.scss";

const TodoListItem = ({ todo, onRemove, onToggle }) => {
  const { id, text, checked } = todo;

  return (
    <div className="TodoListItem">
      <div className={cn("checkbox", { checked })} onClick={() => onToggle(id)}>
        {checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
        <div className="text">{text}</div>
      </div>
      <div className="remove" onClick={() => onRemove(id)}>
        <MdRemoveCircleOutline />
      </div>
    </div>
  );
};
export default TodoListItem;

// TodoInsert.js 
import { useCallback, useState } from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = ({ onInsert }) => {
  const [value, setValue] = useState("");

  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  const onSubmit = useCallback(
    (e) => {
      onInsert(value);
      setValue(""); // value 값 초기화

      // submit 이벤트는 브라우저에서 새로고침을 발생한다. 방지하기 위해 아래코드를 쓴다.
      e.preventDefault();
    },
    [onInsert, value]
  );

  return (
    <form className="TodoInsert" onSubmit={onSubmit}>
      <input
        placeholder="할 일을 입력하세요"
        value={value}
        onChange={onChange}
      />
      <button type="submit">
        <MdAdd />
      </button>
    </form>
  );
};
export default TodoInsert;


// TodoList.js 컴포넌트
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos, onRemove, onToggle }) => {
  return (
    <div className="TodoList">
      {todos.map((todo) => (
        <TodoListItem
          todo={todo}
          key={todo.id}
          onRemove={onRemove}
          onToggle={onToggle}
        />
      ))}
    </div>
  );
};
export default TodoList;


```

 








