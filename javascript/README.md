# JavaScript

## ❓호이스팅에 대해 설명해 주세요.

### 호이스팅

- 호이스팅은 Javascript에서 변수 및 함수의 선언이 스코프의 최상단으로 끌어올려지는 동작을 말합니다. 이로 인해 변수와 함수를 선언하기 전에 사용할 수 있는 것처럼 동작합니다.

- 자바스크립트는 코드를 실행하기 전에 먼저 변수와 함수를 메모리(물리적인 장소x, 논리적인 동작)에 올려놓는데, 이때 선언 부분이 코드 내에서 어디에 위치하든지에 관계없이 해당 스코프의 최상단으로 끌여올려집니다. 하지만 선언만 끌어올려지며, 초기화는 원래 위치엣 이루어집니다.

호이스팅의 예시)

```
// var로 선언된 변수
console.log(myVar); // undefined
var myVar = 10;

// 함수 선언식
myFunction(); // "Hello, world!"
function myFunction() {
  console.log("Hello, world!");
}

// 함수 표현식
myFunction1(); // TypeError: myFunction1 is not a function
var myFunction1 = function() {
  console.log("Hello, world!");
};
```

위의 코드는 사실 아래와 같이 해석됩니다.

```
var myVar;
console.log(myVar); // undefined
myVar = 10;

function myFunction() {
  console.log("Hello, world!");
}

myFunction(); // "Hello, world!"
```

### var/let/const 차이점

let과 const는 "블록 스코프(block-scope)"를 가지기 떄문에 호이스팅이 발생해도 실제로 초기화 되기 전까지 "일시적 사각지대(Temporal Dead Zone: TDZ)"에 빠지게 됩니다.
이는 변수가 선언된 위치에서 해당 변수에 접근할 수 없는 상태를 말합니다.

- 중복 선언 가능: var
- 재할당 가능: var, let
- 중복 선언 불가능: let, const
