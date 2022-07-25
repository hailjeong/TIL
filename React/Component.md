# Component

## 컴포넌트(Component)
  - 컴포넌트는 클래스형과 함수형으로 나뉜다. 
  -  ![class형](https://user-images.githubusercontent.com/101798682/180653289-c59cd5a5-bf11-4884-befa-99b3cd390219.png)
  -  함수형 컴포넌트의 장점 :
    1. 선언하기가 편하다.
    2. 메모리 자원도 클래스형 컴포넌트보다 덜 사용한다.
    3. 배포할 때 함수컴포넌트를 사용하면 결과물의 파일 크기가 더 작다.
    4. hooks를 사용할 수 있다.

- 모듈 내보내기(export), 불러오기(import)
  - export default ... 이 코드는 다른 파일에서 이 파일을 import할 때, 위에서 선언한 클래스르 불러오도록 설정하는 것이다. 
  - import를 하게되면 다른 컴포넌트를 불러올 수 있다. import ... from '해당 파일 경로' 이렇게 작성해주면 된다.

- Props
  - properties를 줄인 표현으로 컴포넌트 속성을 설정할 때 사용하는 요소이다.
  - 리액트 컴포넌트를 사용할 때 태그 사이의 내용을 보여주는 props가 있는데 바로 children이다.\
  -  ![app.js파일](https://user-images.githubusercontent.com/101798682/180710017-0ade225e-7e08-44f6-9ad4-76cc3a23286e.png)
  -  ![MyComponent.js파일](https://user-images.githubusercontent.com/101798682/180710056-fc515e90-3618-4886-a852-fe672bf95b71.png)
  -  이렇게 되면 localhost:3000에서는 <내 이름은 하일 children 값은 리액트> 이런 값이 나온다.
  -  비구조화 할당(destructuring assignment) 함수의 파라미터가 객체라면 그 값을 바로 비구조화 해서 사용한다. 더 짧은 코드로 사용 가능
  - ![비구조화 할당 MyComponent.js](https://user-images.githubusercontent.com/101798682/180714422-d3402fa2-7c50-498d-af4e-46f597a7190e.png)
  - ![더 간단한 방법 MyComponent.js](https://user-images.githubusercontent.com/101798682/180714946-784d2151-da04-4b61-85c9-398a05d87698.png)
  - propTypes를 통한 prop검증
  - propTypes를 지정하는 방법은 defaultProp를 설정하는 것과 비슷하다. import PropTypes from 'prop-types'를 통해 불러와야 한다.
  - isRequired를 사용하여 필수 propTypes로 지정 가능하다. 뒤에 isRequired를 붙여주면 된다. 
  - ![propsTypes](https://user-images.githubusercontent.com/101798682/180717532-fe60213f-c366-4f69-9bdd-a8bccd5156a1.png)
  - PropTypes의 종류는 굉장히 많다. [더 많은 PropTypes 보러가기](https://github.com/facebook/prop-types)

- 클래스형 컴포넌트 props
  - props를 사용할 때 render함수에서 this.props를 조회하면 된다. 나머지 방법은 똑같다. 
  - ![class형 props](https://user-images.githubusercontent.com/101798682/180718417-c37c2905-659f-41dd-ae2a-1fc3e5c22eb0.png)



- ES6 화살표 함수
  - 기존 function을 이용한 함수 선언 방식을 아예 대체 하지는 않는다. 주로 함수를 파라미터로 전달할 때 유용하다.
  ```javaScript
  setTimeout(function() {
    console.log('hello world')
    }, 1000);
    
  setTimeout(() => {
    console.log('hello world')
    }, 1000)
    }
    
  function BlackDog() {
    this.name = '흰둥이';
    return {
      name: '검둥이',
      bark: function () {
        console.log(this.name + ': 멍멍!');
      }
    }
  }
  const blackDog = new BlackDog();
  blackDog.bark();  // 검둥이: 멍멍!
  
  function WhiteDog() {
    this.name = '흰둥이';
    return {
      name: '검둥이',
      bark: () => {
        console.log(this.name + ': 멍멍!');
      }
    }
  }
  const whiteDog = new WhiteDog();
  whiteDog.bark();  // 흰둥이: 멍멍!
  ```
  - 일반 함수는 자신이 종속된 객체를 this로 가리키고 화살표 함수는 자신이 종속된 인스턴스를 가리킨다. 








