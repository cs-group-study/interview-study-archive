# JavaScript

## ❓호이스팅에 대해 설명해 주세요.

### 호이스팅

- 호이스트는 “끌어올린다”라는 뜻입니다. 자바스크립트 코드가 실행될 때 하단에 선언된 변수나 함수가 마치 끌어올려져서 처음부터 상단에 존재했던 것처럼 동작해서 붙여진 이름입니다. 실행 컨텍스트의 생성 단계일 때, 식별자들을 환경 레코드에 미리 기록해놓기 때문에 실행 단계에서 미리 접근할 수 있게 되는 것입니다.

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

## ❓환경 레코드란 무엇인가요?

함수가 실행되면 실행 컨텍스트가 생성되는데, 실행 컨텍스트의 구성요소 중 식별자들을 기록해 놓는 곳이 환경 레코드입니다.

## ❓실행 컨텍스트에 대해 설명해 주세요.

실행 컨텍스트는 자바스크립트 코드를 실행하는 데 필요한 정보들을 모아놓은 객체입니다. 코드가 호출되면 평가 단계를 거치게 되는데 이때 생성되며 아래와 같은 구조를 가지고 있습니다. 

```js
executionContext = {
  variableEnvironment: {
    environmentRecord: {
      // var로 선언된 식별자를 저장
    }
    outerEnvironmentReference: {
      // 외부 lexicalEnvironment를 참조
    }
    thisBinding: // this 값을 저장
  }
  lexicalEnvironment: {
    environmentRecord: {
      // let, const로 선언된 식별자와 함수 선언문을 저장
    }
    outerEnvironmentReference: {
    // 외부 lexicalEnvironment를 참조
    }
    thisBinding: // this 값을 저장
  }
}
```

## ❓전역 코드 평가 과정에 대해 설명해 주세요.

1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
   1. 전역 환경 레코드 생성
      1. 객체 환경 레코드 생성
      2. 선언적 환경 레코드 생성
   2. this 바인딩
   3. 외부 렉시컬 환경에 대한 참조 결정

## ❓variableEnvironment와 lexicalEnvironment의 차이점에 대해 설명해 주세요.

- `variableEnvironment`: var로 선언된 식별자를 저장합니다.
- `lexicalEnvironment`: let, const로 선언된 식별자와 함수 선언문을 저장하며, 다른 실행 컨텍스트의 `outerEnv`로 참조될 수 있습니다.

## ❓다음 코드에서 inner와 outer함수의 참조관계를 outerEnvironmentReference, LexicalEnvironment를 사용해 설명해 주세요.

```js
var a = 1;
function outer() {
  function inner() {
    console.log(a);
    var a = 3;
    console.log(a);
  }
  inner();
  console.log(a);
}
outer();
console.log(a);
```

평가 시점에서의 각 함수 실행 컨텍스트는 다음과 같습니다.

```js
const globalExecutionContext = {
  lexicalEnvironment: {
    environmentRecord: {
      outer: 'function object'
    }
    outerEnv: null
    thisBinding: global
  }
  variableEnvironment: {
    environmentRecord: {
      a: undefined
    }
    outerEnv: null
    thisBinding: global
  }
};

const outerExecutionContext = {
  lexicalEnvironment: {
    environmentRecord: {
      inner: 'function object'
    }
    outerEnv: globalExecutionContext.lexicalEnvironment
    thisBinding: undefined
  }
  variableEnvironment: {
    environmentRecord: {}
    outerEnv: globalExecutionContext.lexicalEnvironment
    thisBinding: undefined
  }
};

const innerExecutionContext = {
  lexicalEnvironment: {
    environmentRecord: {}
    outerEnv: outerExecutionContext.lexicalEnvironment
    thisBinding: undefined
    }
  variableEnvironment: {
    environmentRecord: {
      a: undefined
    }
    outerEnv: outerExecutionContext.lexicalEnvironment
    thisBinding: undefined
    }
};

출력 결과
undefined
3
1
1
```

## ❓environmentRecord로 인해 발생할 수 있는 자바스크립트의 주요 현상은 무엇이 있나요?

실행 컨텍스트의 생성 과정에서 식별자들을 environmentRecord(환경 레코드)에 저장해놓기 때문에 [호이스팅](#호이스팅)이 발생할 수 있습니다.
