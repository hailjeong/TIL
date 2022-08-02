# 컴포넌트 스타일링

- 리액트에서 컴포넌트를 스타일링할 때는 다양한 방식을 사용할 수 있다. 정해진 방식은 없다❗️
- 일반 Css: 컴포넌트를 스타일링하는 가장 기본적인 방식
- Sass: 자주 사용하는 Css 전처리기(pre-processor)중 하나로 확장된 Css문법을 사용하여 Css코드를 더욱 쉽게 작성할 수 있게 해준다.  
- Css Module: 스타일을 작성할 때 Css 클래스가 다른 Css 클래스의 이름과 절대 충돌하지 않도록 파일마다 고유한 이름을 자동으로 생성해주는 옵션
- styled-components: 스타일을 자바스크립트 파일에 내장시키는 방식으로 스타일을 작성함과 동시에 해당 스타일이 적용된 컴포넌트를 만들 수 있게 해준다.

## Css
- Css를 작성할 때 가장 중요한 점은 Css 클래스를 중복되지 않게 만드는 것이다. Css 클래스가 중복되는 것을 방지하는 여러 방식이 있는데, 그 중 하나는 이름을 지을 때 특별한 규칙을 사용하여 짓는 것이고, 하나는 Css Selector를 활용하는 것이다.
- 클래스 이름을 짓는 방식은 다양하다. 예를 들어 컴포넌트 이름-클래스 형태로 짓는 방법이다(App-Header). 클래스 이름에 컴포넌트 이름을 포함시키면서 다른 컴포넌트에서 실수로 중복되는 클래스를 만들어 사용하는 것을 방지할 수 있다.
- Css Selector를 사용하면 Css 클래스가 특정 클래스 내부에 있는 경우에만 스타일을 적용할 수 있다. 예를 들어 .App 안에 들어 있는 .logo에 스타일을 적용하고 싶으면 ```javascript .App .logo { animation: App-logo-spin infinite 20s linear; height: 40vmin;}``` 이렇게 사용할 수 있다.
```javascript
// App.js 컴포넌트
import logo from "./logo.svg";
import "./App.css";

function App() {
  return (
    <div className="App">
      <header>
        <img src={logo} className="logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a href="https://reactjs.org" target="_blank" rel="noopener noreferrer">
          Learn React
        </a>
      </header>
    </div>
  );
}
export default App;

// App.css 컴포넌트
.App {
  text-align: center;
}

/* App 안에 들어 있는 .logo */
.App-logo {
  animation: App-logo-spin infinite 20s linear;
  height: 40vmin;
}
/* .App 안에 들어 있는 header
    header 클래스가 아닌 header 태그 자체에
    스타일을 적용하기 때문에 .이 생략되었습니다. */
.App header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

/* .App 안에 들어 있는 a 태그 */
.App-link {
  color: #61dafb;
}

@keyframes App-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

## Sass 사용하기
- Sass는 복잡한 작업을 쉽게 할 수 있도록 해주고, 스타일 코드의 재활용성을 높여 줄 뿐만 아니라 코드의 가독성을 높여 유지보수를 쉽게 해준다.
- Sass에서는 두 가지 확장자 .scss, .sass를 지원한다. 두 확장자의 문법은 다르다.
- 주요 차이점은 .sass 확장자는 중괄호({})와 세미클론(;)을 사용하지 않는다.
```javascript
// .sass
$front-stack: Helvetica, sans-serif
$primary-color: #333

body
  front: 100% $front-stack
  color: $primary-color

// .scss
$front-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

// SassComponent.js 컴포넌트
import React from "react";
import "./SassComponent.scss";

const SassComponent = () => {
  return (
    <div className="SassComponent">
      <div className="box red" />
      <div className="box orange" />
      <div className="box yellow" />
      <div className="box green" />
      <div className="box blue" />
      <div className="box indigo" />
      <div className="box violet" />
    </div>
  );
};

export default SassComponent;

// SassComponent.scss 컴포넌트
// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

// 믹스인 만들기(재사용되는 스타일 블록을 함수처럼 사용할 수 있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
}

.SassComponent {
  display: flex;
  .box {
    // 일반 Css에서는 .SassComponent .box와 마찬가지
    background: red;
    cursor: pointer;
    transition: all 0.3s ease-in;
    &.red {
      // .red 클래스가 .box와 함께 사용되었을 때
      background: $red;
      @include square(1);
    }
    &.orange {
      background: $orange;
      @include square(2);
    }
    &.yellow {
      background: $yellow;
      @include square(3);
    }
    &.green {
      background: $green;
      @include square(4);
    }
    &.blue {
      background: $blue;
      @include square(5);
    }
    &.indigo {
      background: $indigo;
      @include square(6);
    }
    &.violet {
      background: $violet;
      @include square(7);
    }
    &:hover {
      // box에 마우스 올렸을 때
      background: black;
    }
  }
}
```

## utils 함수 분리하기
- 여러 파일에서 사용될 수 있는 Sass 변수 및 믹스인은 다른 파일로 따로 분리하여 작성한 뒤 필요한 곳에서 쉽게 불러와 사용할 수 있다.
- 다른 Scss 파일을 불러올 때는 @import 구문을 사용합니다.  ```javascript @import "./styles/uitils.scss";```
```javascript
// SassComponent.scss 컴포넌트
@import "./styles/uitils.scss";
.SassComponent {
  display: flex;
  .box {
    // 일반 Css에서는 .SassComponent .box와 마찬가지
    background: red;
    cursor: pointer;
    transition: all 0.3s ease-in;
    &.red {
      // .red 클래스가 .box와 함께 사용되었을 때
      background: $red;
      @include square(1);
    }
    &.orange {
      background: $orange;
      @include square(2);
    }
    &.yellow {
      background: $yellow;
      @include square(3);
    }
    &.green {
      background: $green;
      @include square(4);
    }
    &.blue {
      background: $blue;
      @include square(5);
    }
    &.indigo {
      background: $indigo;
      @include square(6);
    }
    &.violet {
      background: $violet;
      @include square(7);
    }
    &:hover {
      // box에 마우스 올렸을 때
      background: black;
    }
  }
}
```

## Sass-loader 설정
- 반드시 해야 하는 것은 아니지만 알아두면 유용하다. 폴더를 많이 만들어 구조가 깊어졌을 때 상위폴더로 한참 올라가야 하는 단점이 있다.
- 이 문제를 커스터마이징하여 해결할 수 있다. yarn eject 명령어를 통해 세부 설정을 밖으로 꺼내 주어야 한다.
- git에 커밋 되지 않은 변화가 있다면 진행되지 않으니 먼저 커밋을 해야 한다.
- 커밋을 하고 난 뒤, yarn eject를 하고 나면 디렉터리가 생성된다. 그 중에 config 디렉터리를 찾아서 그 안에 webpace.config.js를 열어서 수정을 좀 해주면 된다.
- 아래 코드를 하고 난 뒤, yarn start가 제대로 실행 되지 않으면 node_modules를 지웠다가 다시 yarn install을 해주면 된다.
- 이렇게 하고 나면 우리는 scss컴포넌트에서 ```javascript @import 'utils.scss'``` 이렇게 해주면 된다. 상대경로를 다 안적어도 되서 간편해졌다.
- 더 간단한 방식은 아래코드에서 ```javascript additionalData: "@import 'utils';",``` 이걸 추가해주면 앞으로 항상 utils.scss 파일을 가지고 올 것 이다.
```javascript
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders({
  importLoaders: 3,
  sourceMap: isEnvProduction
  ? shouldUseSourceMap
  : isEnvDevelopment,
  }).concat({
  loader: require.resolve("sass-loader"),
  options: {
  sassOptions: {
  includePaths: [paths.appSrc + "/styles"],
  },
  additionalData: "@import 'utils';",
  },
  }),
  sideEffects: true,
  },
```

## node_modules에서 라이브러리 불러오기
- Sass의 장점 중 하나는 라이브러리를 쉽게 불러와서 사용할 수 있다는 점이다.
- 상대경로를 적을 때 많은 디렉터리에 위치할 경우 ../를 매우 많이 적어야 해서 번거롭다. 그럴땐 물결 문자를 사용하는 것이다. (~) ```javascript @import '~library/styles';``` 
- 물결 문자를 사용하면 자동으로 node_modules에서 라이브러리 디렉터리를 탐지해 스타일을 불러올 수 있다.
```javascript
// utils.scss 컴포넌트
@import "~include-media/dist/include-media";
@import "~open-color/open-color";

// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

// 믹스인 만들기 (재사용되는 스타일 블록을 함수처럼 사용 할 수 있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
}

// SassComponent.scss 컴포넌트
.SassComponent {
  display: flex;
  background: $oc-gray-2;
  @include media("<768px") {
    background: $oc-gray-9;
  }
```

## Css Module
- Css를 불러와서 사용할 때 클래스 이름을 고유한 값, 즉 [파일 이르]_[클래스 이름]_[해시값] 형태로 자동으로 만들어서 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해 주는 기술이다.
- 업데이트가 되고 난 뒤 버전2 이상부터 별도의 설정이 필요없다. .module.css확장자로 파일을 저장하기만 하면 Css Module이 적용 된다.
- 웹 페이지에서 전역적으로 사용되는 경우라면 :global을 앞에 입력하여 글로벌 Css임을 명시해 줄 수 있다.
- CSS Module을 사용한 클래스 이름을 두 개 이상 적용할 때는 추가 부분을 참고하면 된다.
```javascript
//  CssModule.module.css 컴포넌트
/* 자동으로 고유해질 것이므로 흔히 사용되는 단어를 클래스 이름으로 마음대로 사용 가능 */
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

/* 글로벌 css 작성하고 싶다면 */
:global .something {
  font-weight: 800;
  color: aqua;
}

// CSSModule.js 컴포넌트
import styles from "./CssModule.module.css";
const CSSModule = () => {
  return (
    <div className={styles.wrapper}>
      안녕하세요, 저는<sapn className="something">CSS Module</sapn>
    </div>
  );
};
export default CSSModule;

// 4번째 참고 코드
// CssModule.module.css 컴포넌트
/* 자동으로 고유해질 것이므로 흔히 사용되는 단어를 클래스 이름으로 마음대로 사용 가능 */
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

.inverted {
  color: black;
  background: white;
  border: 1px solid black;
}

/* 글로벌 css 작성하고 싶다면 */
:global .something {
  font-weight: 800;
  color: aqua;
}
// CSSModule.js 컴포넌트 위에 코드랑 똑같은데 이 부분만 수정해 주면 된다.
<div className={`${styles.wrapper} ${styles.inverted}`}>
```

## classnames
- classnames는 CSS 클래스를 조건부로 설정할 때 매우 유용한 라이브러리이다.
- classnames에 내장되어 있는 bind함수를 사용하면 클래스를 넣어 줄 때마다 styles.[클래스이름]형태를 사용할 필요가 없다. cx('클래스 이름1', '클래스 이름2')아래에 예제 코드를 참고하면 된다.
- 클래스를 여러개 설정하거나, 조건부로 클래스를 설정할 때 classnames의 bind함수를 사용하면 더욱 편리하다.
```javascript
import classNames from 'classnames/bind';
import styles from './CSSModule.module.css';

const cx = classNames.bind(styles); // 미리 styles에서 클래스를 받아 오도록 설정하고

const CSSModule = () => {
  return (
    <div className={cx('wrapper', 'inverted')}>
      안녕하세요, 저는 <span className="something">CSS Module</span>
    </div>
  );
};
export default CSSModule;
```

## Sass와 함께 사용하기
- Sass를 사용할 때 이름 뒤에 .module.scss 확장자를 사용해주면 CSS Module로 사용할 수 있다.
```javascript
// CSSModule.module.scss 컴포넌트

/* 자동으로 고유해질 것이므로 흔히 사용되는 단어를 클래스 이름으로 마음대로 사용 가능 */
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
  &.innverted {
    // inverted가 .wrapper와 함께 사용되었을 때만 적용
    color: black;
    background: white;
    border: 1px solid black;
  }
}

/* 글로벌 css 작성하고 싶다면 */
:global {
  // :global {}로 감싸기
  .something {
    font-weight: 800;
    color: aqua;
  }
  // 여기에 다른 클래스를 만들 수 도 있다.
}
```

## styled-components
- 컴포넌트 스타일링의 또 다른 패러다임은 자바스크림트 파일 안에 스타일을 선언하는 방식이다. => CSS-in-JS 이걸 styled-components라고 한다. 라이브러리 중 emotion-styled가 가장 대표적이다. 그래서 수업시간에 emotion으로만 배웠다❗️
- 템플릿 리터럴
  - 백틱을 이용한 방법인데 템플릿 안에 자바스크립트 객체나 함수를 전달 할 때 온전히 추출할 수 있다. 
```javascript
// StyledComponent.js 컴포넌트
import React from "react";
import styled, { css } from "styled-components";

const sizes = {
  desktop: 1024,
  tablet: 768,
};

// 위에있는 size 객체에 따라 자동으로 media 쿼리 함수를 만들어줍니다.
// 참고: https://www.styled-components.com/docs/advanced#media-templates
const media = Object.keys(sizes).reduce((acc, label) => {
  acc[label] = (...args) => css`
    @media (max-width: ${sizes[label] / 16}em) {
      ${css(...args)};
    }
  `;

  return acc;
}, {});

const Box = styled.div`
  /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
  background: ${(props) => props.color || "blue"};
  padding: 1rem;
  display: flex;
  width: 1024px;
  margin: 0 auto;
  ${media.desktop`width: 768px;`}
  ${media.tablet`width: 100%;`};
`;

const Button = styled.button`
  background: white;
  color: black;
  border-radius: 4px;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  font-size: 1rem;
  font-weight: 600;
  /* & 문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
  &:hover {
    background: rgba(255, 255, 255, 0.9);
  }
  /* 다음 코드는 inverted 값이 true 일 때 특정 스타일을 부여해줍니다. */
  ${(props) =>
    props.inverted &&
    css`
      background: none;
      border: 2px solid white;
      color: white;
      &:hover {
        background: white;
        color: black;
      }
    `};
  & + button {
    margin-left: 1rem;
  }
`;

const StyledComponent = () => (
  <Box color="black">
    <Button>안녕하세요</Button>
    <Button inverted={true}>테두리만</Button>
  </Box>
);

export default StyledComponent;
```
## 정리🧑‍💻
- 리액트에서는 대부분 emotion 라이브러리를 사용하는게 대표적이다. 수업시간에 배운걸 기억하고 복습하고 이것저것 많이 찾아보고 하자
- 























