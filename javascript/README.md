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

## ❓클로저에 대해 설명해주세요.

- 클로저는 자바스크립트 뿐만이 아니라 함수형 프로그래밍 언어에서 흔히 사용되는 개념으로, 함수와 해당 함수가 선언된 렉시컬 환경(Lexical Environment) 사이의 관계를 나타냅니다. 내부 함수가 유효한 상태에서 외부 함수가 종료되어 외부 함수의 실행 컨텍스트가 반환되어도, 외부 함수 실행 컨텍스트 내의 변수는 내부 함수에 의해 참조되는 한 유효하여, 내부 함수가 스코프 체인을 통해 참조할 수 있다는 것을 의미합니다.

- 즉, 외부 함수가 이미 반환되었어도, 외부 함수 내의 변수는 이를 필요로 하는 내부 함수가 하나 이상 존재하는 경우 계속 유지되고, 실제 변수에 접근 할 수 있습니다.

## ❓스코프 체인에 대해 설명해 주세요.

- 스코프(scope)는 식별자에 대한 유효 범위를 뜻하며, 스코프 체인(scope chain)은 식별자의 유효 범위를 안에서부터 바깥으로 차례대로 검색해 나가는 것을 말합니다.

- 이를 가능하게 하는 것이 outerEnvironmentReference인데, outerEnvironmentReference는 현재 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조합니다. 이는 연결 리스트의 형태를 띠며, 선언 시점의 LexicalEnvironment를 계속 찾아 올라가면 마지막에 전역 건텍스트의 LexicalEnvironment가 있습니다.

- 이러한 구조적 특성때문에, 가장 가까운 요소부터 차례대로만 접근할 수 있고, 다른 순서로 접근하는 것이 불가능합니다. 즉, 여러 스코프에서 동일한 식별자를 선언한 경우에는 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능합니다.

## ❓클로저를 직접적으로 활용한 사례나 알고있는게 있다면 설명해주세요.

간단한 예를 들어보자면, DOM 요소의 표시 상태를 토글을 통해 변환시키는 함수를 작성할 때, 해당 토글 함수에, 표시 상태를 나타내는 변수를 선언, 이후 해당 변수를 토글 시키는 클로저를 반환함으로써 표시 상태를 나타내는 변수를 숨기면서도, 최신 상태를 계속 유지하고, 변경될 수 있게 할 수 있습니다.

이렇게 전역변수를 사용하지 않고도, 최신 상태를 유지하고 변경또한 가능하게 하면서 코드의 의도되지 않은 변경을 예방할 수 있습니다.

실제로 리액트의 useState 또한 클로저를 통해 구현되어 있습니다. useState는 state, setState를 반환하게 되는데 해당 setState가 실행될 때 이전 상태와 현재 상태의 변경을 감지해야 하고, 그러기 위해 이전 상태를 가지고 있어야 하는데 이 곳에서 클로저가 활용됩니다.

```javascript
const useState = (value) => {
  let saveValue = value;

  const state = () => saveValue;

  const setState = (newValue) => {
    saveValue = newValue;
  };

  return [state, setState];
};
```

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

## ❓this에 대해 설명해 주세요.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수입니다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있습니다. this와 this가 가리키는 값을 연결하는 것을 this 바인딩이라고 하는데 함수 호출 방식에 의해 동적으로 결정됩니다.

## ❓함수 호출 방식에 따라 달라지는 this 바인딩에 대해 설명해 주세요.

### 일반 함수 호출

일반 함수 내부에서 `this`는 전역 객체를 가리킵니다. (`strict mode`에서는 `undefined`)

```js
function func() {
  console.log(this);
}

func(); // window

(function func() {
  'use strict';
  console.log(this); // undefined
})();
```

### 메서드 호출

객체의 메소드로 호출될 때, `this`는 해당 객체를 가리킵니다.

```js
const obj = {
  method: function () {
    console.log(this);
  },
};

obj.method(); // obj
```

### 생성자 함수 호출

생성자 함수로 호출될 때, `this`는 새로 생성된 객체(인스턴스)를 가리킵니다.

```js
function Person(name) {
  this.name = name;
}

const john = new Person('John');
console.log(john.name); // "John"
```

### apply, call, bind에 의한 호출

#### `apply`

- 함수를 호출하는 함수.
- `함수.apply(thisArg, [arg1, ..., argN])`
- `this`에 바인딩 할 객체와 함수에 전달할 인자를 `배열(유사배열)`에 넣어서 실행한다.

#### `call`

- 함수를 호출하는 함수.
- `함수.call(thisArg, arg1, ..., argN)`
- `this`에 바인딩 할 객체와 함수에 전달할 인자를 넣어서 실행한다.

#### `bind`

- `apply`나 `call`과 다르게 함수를 실행시키지 않고 `this`가 바인딩 된 함수를 반환한다.
- `함수.bind(thisArg, arg1, ..., argN)`
- `this`에 바인딩 할 객체와 함수에 전달할 인자를 전달한다.
- [`bind`는 함수처럼 호출 가능한 `특수 객체(exotic object)`를 반환한다.](https://ko.javascript.info/bind#ref-605)
- 이 특수 객체를 호출하면 `this`가 `bind`에 넘겨준 객체로 고정된 함수가 호출된다.

```js
function countNumbers(a, b) {
  console.log(`${this.name}: ${a}, ${b}`);
}

const person = { name: 'John' };
countNumbers.apply(person, [1, 2]); // "John: 1, 2"
countNumbers.call(person, 1, 2); // "John: 1, 2"
countNumbers.bind(person)(1, 2); // "John: 1, 2"

const anotherPerson = { name: 'Smith' };
countNumbers.bind(person).call(anotherPerson, 1, 2); // "John: 1, 2" => bind로 인해 this가 person으로 고정됨
```

## ❓어떤 함수의 내부에서 호출된 함수의 this가 전역객체를 참조하는 것을 회피하려면 어떻게 해야 하나요?

- `this`를 변수에 할당해두고 내부 함수에서 참조하는 방법

```js
const obj = {
  method: function () {
    const memoizedThis = this; // this를 변수에 할당
    console.log(this); // obj

    function innerFunc() {
      console.log(this); // window
      console.log(memoizedThis); // obj
    }
    innerFunc();
  },
};

obj.method();
```

- 화살표 함수를 사용하는 방법

```js
const obj = {
  method: function () {
    console.log(this); // obj

    const innerFunc = () => {
      console.log(this); // obj
    };
    innerFunc();
  },
};

obj.method();
```

- `apply`, `call`, `bind`를 사용해서 명시적으로 바인딩하는 방법

```js
const obj = {
  method: function () {
    console.log(this); // obj

    function innerFunc(arg1, arg2) {
      console.log(this); // obj
    }
    innerFunc.apply(obj, [1, 2]);
    innerFunc.call(obj, 1, 2);
    innerFunc.bind(obj)(1, 2);
  },
};

obj.method();
```

## ❓setTimeout, forEach, addEventListener에 넘겨준 콜백 함수에서 this는 어떻게 바인딩 될까요?

콜백 함수가 일반 함수인지 화살표 함수인지에 따라 다릅니다.

### setTimeout

아래의 `normalTimer`와 `arrowTimer` 모두 객체 `obj`의 메서드로 호출되었으므로 `this`는 `obj`에 바인딩됩니다.  
`setTimeout`의 콜백함수는 형태에 따라 다른데, `setTimeout`의 콜백함수는 콜스택이 전부 빈 뒤에야 이벤트 루프에 의해 콜스택에 진입합니다.  
일반함수의 경우 호출될 때의 `this`는 `window`에 바인딩됩니다.  
화살표함수의 경우 상위 스코프의 `this`에 바인딩되므로 `obj`에 바인딩됩니다.

```js
const obj = {
  // 일반
  normalTimer() {
    console.log(this); // obj
    setTimeout(function () {
      console.log(this); // window
    }, 0);
  },
  // 화살표
  arrowTimer() {
    console.log(this); // obj
    setTimeout(() => {
      console.log(this); // obj
    }, 0);
  },
};

obj.normalTimer();
obj.arrowTimer();
```

### forEach

아래의 `normalForEach`와 `arrowForEach` 모두 객체 `obj`의 메서드로 호출되었으므로 `this`는 `obj`에 바인딩됩니다.  
`forEach`의 콜백함수는 형태에 따라 다른데,  
일반함수의 경우 호출될 때의 `this`는 `window`에 바인딩됩니다.  
화살표함수의 경우 상위 스코프의 `this`에 바인딩되므로 `obj`에 바인딩됩니다.

```js
const obj = {
  arr: [0],
  // 일반
  normalForEach() {
    console.log(this); // obj
    this.arr.forEach(function () {
      console.log(this); // window
    });
  },
  // 화살표
  arrowForEach() {
    console.log(this); // obj
    this.arr.forEach(() => {
      console.log(this); // obj
    });
  },
};

obj.normalForEach();
obj.arrowForEach();
```

### addEventListener

`addEventListner`의 콜백함수가 일반함수일 경우 `this`는 이벤트가 발생한 요소에 바인딩됩니다. 하지만 화살표함수일 경우 상위 스코프인 `window`에 바인딩됩니다.

```js
// 일반
target.addEventListener('click', function (e) {
  console.log(this); // target === e.target
});
// 화살표
target.addEventListener('click', (e) => {
  console.log(this); // window
});

target.click();
```

## ❓함수 선언식과 함수 표현식의 차이점이 무엇인가요?

### 함수 선언식

- `function` 키워드를 사용해서 함수를 만드는 방식
- 일반적으로 기명함수(이름이 있는 함수)로 정의
- 함수 호이스팅이 적용됨

### 함수 표현식

- 함수를 일급객체로 취급하는 자바스크립트의 특징을 활용해 변수에 함수를 할당하는 방식
- 익명함수(이름이 없는 함수)로 정의될 수도 있음
- 주로 한 번만 사용되거나 다른 함수 내에서 사용될 때 유용
- 변수 호이스팅이 적용됨

## ❓함수와 메서드는 무엇이 다른가요?

- **함수**: 특정한 작업을 수행하는 기능 단위의 코드
- **메서드**: 객체의 속성 또는 클래스의 멤버로 할당된 함수

## ❓빌트인 객체가 무엇인가요? 또 종류는 어떤 것들이 있나요?

빌트인 객체(Built-in Object)는 JavaScript 언어 자체에 내장된 객체로 기본적인 데이터 유형을 다루거나 특정 작업을 수행하기 위한 객체입니다.자바스크립트 엔진에 내장되어 사용자의 환경에 상관 없이 즉시 사용할 수 있는 코드를 의미합니다.

### 기본 데이터 유형과 관련된 빌트인 객체

`Object` 모든 객체의 기본이 되는 객체.
`Array` 배열을 다루는 객체.
`String` 문자열을 다루는 객체.
`Number` 숫자를 다루는 객체.
`Boolean` 불리언 값을 다루는 객체.
`Symbol` 유일한 값을 나타내는 심볼 값을 다루는 객체.

### 예외 처리와 관련된 빌트인 객체

`Error`
`AggregateError`
`EvalError`
`RangeError`
`ReferenceError`
`SyntaxError`
`TypeError`
`URIError`

### 숫자와 날짜와 관련된 빌트인 객체

`Number`
`BigInt`
`Math`
`Date`

### DOM(Document Object Model) 다루기와 관련된 빌트인 객체

`document` 웹 페이지의 DOM을 조작하는 객체.
`window` 브라우저 창을 나타내는 객체.

### 전역 객체(Global Object)

`global` 전역 범위에서(Node.js 환경) 사용할 수 있는 객체.
