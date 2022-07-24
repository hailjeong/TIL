# Component

- 컴포넌트(Component)
  - 컴포넌트는 클래스형과 함수형으로 나뉜다. 
  -  ![class형](https://user-images.githubusercontent.com/101798682/180653289-c59cd5a5-bf11-4884-befa-99b3cd390219.png)
  -  함수형 컴포넌트의 장점 :
    1. 선언하기가 편하다.
    2. 메모리 자원도 클래스형 컴포넌트보다 덜 사용한다.
    3. 배포할 때 함수컴포넌트를 사용하면 결과물의 파일 크기가 더 작다.
    4. hooks를 사용할 수 있다.

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








