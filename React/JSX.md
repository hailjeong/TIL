# JSX

- 번들러(Bundler)
  - 먼저 터미널에서 yarn create react-app <프로젝트 명>을 통해 작업 환경을 구축해준다.
  - 파일을 만들어준 이후 VS Code에서 해당 파일을 열어준다. App.js에 들어가면 import를 사용해 다른 파일들을 불러온 걸 볼 수 있다. 이건 Node.js에서 지원하는 기능이다. 
  - 이러한 기능을 브라우저에서도 사용하기 위해 번들러(bundler)를 사용한다. 파일을 묶듯이 연결하는 거라고 생각하면 된다.
- 웹팩
  - 대표적인 번들로로는 웹팩, Parcel, browserify라는 도구들이 있지만 리액트 프로젝트에서는 주로 웹팩을 사용하는 추세이다. 그 이유는 편의성과 확장성이 다른 도구보다 뛰어나기 때문이다. 웹팩을 사용하면 SVG파일과 CSS파일도 불러와서 사용할 수 있다. 이렇게 파일들을 불러오는것은 웹팩의 로더(loader)라는 기능이 담당한다.
  - 로더는 여러 가지 종류가 있다. css-loader는 Css 파일을 불러올 수 있게 하고 file-loader는 웹 폰트나 미디어 파일등을 불러올 수 있게 해준다. babel-loader는 자바스크립트 파일들을 불러오면서 최신 자바스크립트 문법으로 작성된 코드를 바벨이라는 도구를 사용하여 ES5문법으로 변환해준다.
  - 여기서 왜 이전 버전의 자바스크립트 문법으로 바꾸냐는 생각을 하게 되었는데 책에서 아주 자세히 설명이 되어 있었다. 이러한 이유는 구버전 웹 브라우저와 호환하기 위해서이다.

- JSX란
  - JSX는 자바스크립트의 확장 문법이며, 브라우저에서 실행되기 전에 코드가 번들링 되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다.
  ```JavaScript
  function App() {
    return (
      <div>
        Hello Hail
      </div>
    );
  }
  ```
  ```JavaScript
  function App() {
    return React.createElement("div", null, "Hello hail")
  }
  ```
  
  - JSX 장점
    - 가독성이 높고 작성하기도 쉽다.
    - HTML 태그를 사용할 수 있을 뿐만 아니라, 앞으로 만들 컴포넌트도 JSX 안에서 작성할 수 있다.

  - ReactDOM.render
    - 컴포넌트를 페이지에 렌더링하는 역할을 하며, react-dom 모듈을 불러와 사용할 수 있다.
  - React.StricMode
    - 리액트 프로젝트에서 리액트의 레거시 기능들을 사용하지 못하게 하는 기능이다. 사용하면 문자열 ref, componentWillMount등 나중에 사라지게 될 옛날 기능을 사용했을 때 경고를 출력한다.

  - JSX 문법
    - 컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야 한다. 
    - 부모 요소로 감쌀때 리액트 v16이상부터 도입된 Fragment라는 기능을 사용하면 된다. (<> </>) 이것도 사용 가능하다.
    - 자바스크립트 표현식을 작성하려면 코드를 {}로 감싸야 한다.
      ```JavaScript
        function App() {
          const name = '하일';
        return (
          <>
            <h1>{name}안녕</h1>
          </>
          );
        }
        export default App;
      ```
      
  - If문 대신 조건 연산자(삼항연산자)
    - JSX내부에서 if문을 사용할 수 없다. JSX 밖에서 if문을 사용해 사전에 값을 설정하거나, {}안에 조건부 연산자를 사용해야 한다. 조건부 연산자 또 다른 이름은 삼항 연산자 이다.
    ```javascript
    function App() {
      const name = "하일";
    return (
      <div>{name === "하일" ? <h1>하일이 맞다</h1> : <h2>하일이 아니다</h2>}</div>
     );
    }
    export default App;
    ```
  - && 연산자를 이용한 조건부 렌더링
    - 특정 조건을 만족할 때 내용을 보여주고, 만족하지 않으면 아예 아무것도 렌더링 하지 않아야 하는 상황에서 사용한다.
    - && 연산자로 조건부 렌더링을 할 수 있는 이뉴는 리액트에서 false를 렌더링할 때는 null과 마찬가지로 아무것도 나타나지 않기 때문이다.
    - 이때 주의해야 할 점은 falsy한 값인 0은 예외적으로 화면에 나타난다는 것이다.
    ```javascript
    function App() {
      const name = "하일";
    return <div>{name === "하일" && <h1>하일</h1>}</div>;
    }
    export default App;
    ```
  - undefined
    - 어떤 값이 undefined일 수도 있다면 OR연산자(||)를 사용해 오류를 방지할 수 있다.
  - 인라인 스타일링
    - 스타일을 css파일이 아닌 JSX안에서 줄려고 할 때 background-color 이러한것들은 가운데 - 를 빼고 카멜 표기법으로 작성한다. backgroundColor  
  - className
    - HTML에서는 class를 사용했지만 JSX에서는 className을 사용한다.
    - ![className_Css](https://user-images.githubusercontent.com/101798682/180606955-76e3358f-c62e-4481-a411-ab66e6661dc1.png)
    - ![className_JSX](https://user-images.githubusercontent.com/101798682/180606974-c70e5957-bbda-4b7e-a417-b26fe206fd03.png)
  - 주석처리
    - JSX에서 주석 처리를 할 때에는 // 이게 아닌 {/* ... */} 이런 방식을 사용한다. 
     


- ES6의 const let
  - ES6에서 const가 새로 도입 되면서 기존에 사용되던 var는 거의 사용할 일이 없게 됐다. 먼저 var를 사용하지 않게 된 이유는 scope(해당 값으 사용할 수 있는 코드 영역)가 함수 단위이다.
  - 호이스팅이 일어나면서 값이 바뀌게 된다.
  - let과 const는 호이스팅이 일어나지 않으면서 값이 바뀌진 않는다.
  - let은 변수로 값이 바뀌는 것을 말한다. 하지만 이건 scope가 블록 단위이다.
  - const는 상수로 한 번 선언하고 나면 값이 변경될 수 없다. 
