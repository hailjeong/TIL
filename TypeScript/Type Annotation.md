# Type Annotation
- 타입을 지정해주는 것을 Type Annotation 이라고 부른다. 지정해 주는 방법은 아래 코드를 참고할 수 있다.
- 아래 코드처럼 작성했을 경우 a = 39에는 빨간 줄(에러)이 뜨게 되는데 그 이유는 위에서 a를 string으로 지정해줬기 때문이다.
- 두 번째 함수를 봤을 때에도 에러가 나오는데 그 이유는 b: number로 지정해줬기 때문에 hello() 안에 문자열이 올 수 없다.

```js
let a: string;

a = "hail";

a = 39;

function hello(b: number) {
  
}

hello("hail");
```
