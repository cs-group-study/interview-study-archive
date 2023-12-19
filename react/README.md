# React

## ❓useState가 어떻게 동작하는지 설명해 주세요.

### React의 state가 상태를 기억하는 방법

React에서 상태(State) 관리는 javascript의 클로저 개념을 활용한 패턴 중 하나입니다.
React의 컴포넌트는 useState hook을 이용해 상태관리를 합니다. useState Hook은 클로저를 이용하여 동작하며, 이를 통해 각 컴포넌트 인스턴스마다 독립적인 상태를 가질 수 있습니다.

한 컴포넌트가 있다고 가정했을 때, 컴포넌트가 렌더링 될 때마다 '함수형 컴포넌트' 함수가 호출되지만, 해당 함수(컴포넌트) 내에서 선언된 변수들은 클로저를 통해 이전 값이 유지되고 상태 관리가 가능해집니다.
따라서 React의 상태관리는 내부적으로 클로저를 활용한 컴포넌트 간 상태를 격리하고 관리하는 기능을 제공합니다.

\*클로저: 클로저란 함수와 그 함수가 선언되었을 때 Lexical Scope 사이의 관계를 나타내는 개념. 클로저를 사용하면 함수 내부에서 외부 스코프의 변수에 접근할 수 있다.

## ❓리액트에서 Hook은 무엇인가요? 어떤 리액트 hook을 쓰고 있나요?

React 라이브러리에서 함수 컴포넌트 내에서 상태 관리와 다양한 기능을 사용하기 위한 방식입니다.
React Hook은 크게 내장 Hook과 커스텀 훅을 만들어 사용하는 두 가지 방법이 있습니다.

React Hook은 React 버전 16.8에서 도입된 기능으로, 함수형 컴포넌트에서도 상태(state)와 생명주기(lifecycle) 기능을 사용할 수 있도록 해줍니다. 클래스형 컴포넌트에서만 사용되던 기능들을 함수형 컴포넌트에서도 사용할 수 있게 만들어 React 코드를 간결하고 재사용 가능하게 만들어 줍니다.

### Hooks 이점들

1. **코드 간결성**: 함수형 컴포넌트와 Hooks를 사용하면 클래스 컴포넌트보다 코드가 더 간결해집니다. 따라서 코드를 이해하고 유지 보수하기가 더 쉬워집니다.
2. **재사용성**: 클래스 컴포넌트에서 로직 재사용하려면 고차 컴포넌트(HOC) 패턴을 사용해야 했으나, 커스텀 Hooks를 만들어 여러 컴포넌트에서 재사용할 수 있습니다. 이로 인해 코드의 중복을 줄이고 논리를 쉽게 공유할 수 있습니다.
3. **컴포넌트 분리**: Hooks를 이용하면 상태 관련 로직을 컴포넌트에서 분리하여 더욱 독립적인 함수로 만들 수 있습니다. 이로 인해 컴포넌트를 더욱 재사용 가능하고 테스트하기 쉬운 구조로 만들 수 있습니다.
4. **라이프사이클 메서드 문제 해결**: 클래스 컴포넌트에서는 관련 있는 코드가 여러 라이프사이클 메서드에 분산될 수 있습니다. 이로 인해 코드의 일관성을 유지하거나 버그를 잡기 어려울 수 있습니다. Hooks를 사용하면 이런 문제를 해결하고, 관련 있는 코드를 한데 묶을 수 있습니다.

### 리액트에서의 불변성

리액트에서 상태의 불변성을 지켜야 하는 이유는 기본적으로 상태의 변경이 예측 가능하고 일관성 있게 이뤄져야 하기 때문이다. 또한 리렌더링 될 경우 리액트는 Hooks가 호출되는 순서를 통해 상태를 추적하고 관리하는데, 불변성을 안 지키게 되면 다른 값으로 판단하기 때문에 새로운 값을 생성하게 되고 이는 성능 저하로 이어진다.

### Hook과 일반 함수와의 차이점은?

1. Hook은 최상위 레벨에서 호출되어야 한다.

- 조건문, 반복문, 중첩 함수 내에서는 호출되면 안됩니다.

2. 컴포넌트 내에서만 호출되어야 한다.

- Hook은 함수 컴포넌트 내에서만 호출되어야 합니다. 일반 JavaScript 함수나 클래스 내부에서 호출되면 안됩니다.

또한 React Hooks는 함수형 컴포넌트에서 state 및 life cycle 메서드와 같은 기능을 제공한다.

## ❓React hooks의 장점은 무엇인가요?

React Hooks는 함수형 컴포넌트에서 상태와 생명주기 기능을 관리하기 위한 기능으로 도입한 것인데, 효율적이고 유연한 개발을 할 수 있게 해줍니다.

- 가독성과 간결성 향상
  - Hooks를 사용하면 Class형 컴포넌트에서 사용되던 복잡한 구조와 문법을 피할 수 있습니다.
- 성능 최적화
  - Hooks는 리액트 내부에서 최적화가 더 잘 이루어질 수 있도록 도와줍니다. 예를 들어 **`useMemo`**와 **`useCallback`**은 불필요한 렌더링을 방지하고 성능을 향상시킬 수 있습니다.
- 테스트 용이성
  - 함수형 컴포넌트와 Hooks를 사용하면 단위 테스트 작성이 더 쉬워집니다. Hooks를 이용하여 컴포넌트의 로직을 테스트하기가 더 간단하고 직관적입니다.

### 커스텀 훅

커스텀 훅을 만들어 사용하는 주요 이점 중 하나는 **로직의 분리와 추상화**입니다. (\*추상화란 복잡한 시스템으로부터 핵심적인 개념또는 기능을 간추려내는 것을 의미) UI와 상태(state)를 분리하여 커스텀 훅으로 감싸면, 컴포넌트 코드는 더 간결해지고 가독성이 높아집니다. 또한 같은 로직을 여러 컴포넌트에서 사용할 때 중복을 피하고 일관성 있게 코드를 작성할 수 있습니다.

## ❓useRef의 타입에 따라 달라지는 사용법에 대해 설명해 주세요.

`useRef`의 타입에는 세가지가 있습니다.

### 1. useRef<T>(initialValue: T): MutableRefObject<T>;

```tsx
function useRef<T>(initialValue: T): MutableRefObject<T>;
```

제네릭 타입과 `initialValue`의 타입이 `T`로 일치하는 경우, `MutableRefObject<T>` 타입의 객체를 반환하는데, 객체의 속성인 `current`의 값을 변경(mutate)할 수 있습니다.

```ts
예시)
const value = useRef<number>(0);
value.current = 1;
```

### 2. useRef<T>(initialValue: T | null): RefObject<T>;

```ts
function useRef<T>(initialValue: T | null): RefObject<T>;
```

`initialValue`의 타입이 `null`을 허용하는 경우, `RefObject<T>` 타입의 객체를 반환하는데, 객체의 속성인 `current`의 값을 변경(mutate)할 수 없습니다.

```ts
예시)
const value = useRef<number>(null);
value.current = 1; // Cannot assign to 'current' because it is a read-only property.
```

주로 DOM 객체에 접근할 때 사용합니다.

```ts
예시)
const inputRef = useRef<HTMLInputElement>(null)
inputRef.current.value = 'hi'
```

여기서 `current`의 `value`를 변경할 수 있는 이유는 `current`가 read-only인 것이지 `current`의 하위 속성들은 관계없기 때문입니다.

`current`의 값을 변경 가능하게 만들고 싶다면 제네릭의 인자에 `| null` 을 포함시키면 됩니다. 이때 반환 객체의 타입은 `MutableRefObject<number | null>`로 변경됩니다.

```ts
예시)
const value = useRef<number | null>(null);
value.current = 1;
```

### 3. useRef<T = undefined>(): MutableRefObject<T | undefined>;

```ts
function useRef<T = undefined>(): MutableRefObject<T | undefined>;
```

`useRef`에 초깃값을 넣지 않은 경우, `MutableRefObject<T | undefined>` 타입의 객체를 반환합니다.

```ts
예시)
const value = useRef<number>(); // MutableRefObject<number | undefined>
value.current = 1;
value.current = undefined;
```

## ❓lazy initialization(게으른 초기화)에 대해 설명해 주세요.

useState에 직접적인 값 대신에 함수를 넘기는 것을 게으른(lazy) 초기화라고 합니다. 게으른 초기화 함수는 state가 처음 만들어 질 때만 실행되고 나중에 다시 리렌더링이 된다면 이 함수의 실행은 무시됩니다. 상태의 초깃값을 localStorage에서 가져오거나 map, filter, find 등 복잡한 연산으로 구해야 하는 경우에 사용합니다.

## ❓코드 스플리팅과 게으른 초기화에는 어떤 차이점이 있나요?

둘 다 최적화 기법이지만 사용법과 사용목적이 다릅니다. 코드 스플리팅은 `React.lazy(() => import('컴포넌트 경로'))`를 사용해 지금 당장 필요하지 않은 컴포넌트를 번들에 포함시키지 않고 필요한 시점에 로드하는 기법입니다. 게으른 초기화는 상태의 초깃값으로 함수를 넘겨서 상태의 최초 생성 시점에 한 번만 생성하는 기법입니다.

## ❓useLayoutEffect에 대해 설명해 주세요.

`useEffect`와 `useLayoutEffect`는 비슷하지만 실행 시점이 다릅니다. `useEffect`는 레이아웃과 페인트 이후에 실행되고 `useLayoutEffect`는 DOM이 업데이트 된 뒤에 실행됩니다. 따라서, `useEffect`를 사용하면 시각적인 깜빡임이 발생할 수 있습니다. 하지만, `useLayoutEffect`를 사용하면 화면이 그려지기 전에 작업이 처리되기 때문에, 이러한 깜빡임을 방지할 수 있습니다.

`useLayoutEffect`는

1. DOM 노드에 대한 작업이 필요한 경우
2. 브라우저의 렌더링이 완료되기 전에 작업이 처리되어야 하는 경우
3. 시각적인 깜빡임을 방지해야 하는 경우에 유용합니다.

다만, `useLayoutEffect`는 화면이 그려지기 전에 실행되기 때문에, 작업의 처리 시간이 매우 긴 경우, 사용자가 빈화면을 오랫동안 보게될 수 있으므로 주의해야 합니다.

## ❓useMemo, useCallback에 대해 설명해 주세요.

- `useMemo`는 비용이 높은 연산의 결괏값를 캐시하고, 의존성이 변경될 때만 다시 연산해서 성능을 향상시키는 데 사용됩니다. 특히 계산량이 크거나 렌더링 중에 자주 호출되는 계산의 경우에 유용합니다.
- `useCallback`은 주로 함수의 참조 안정성을 위해 사용됩니다. 함수를 컴포넌트 내에서 만들면, 해당 컴포넌트가 리렌더링될 때마다 새로운 함수가 생성되어서 참조값이 달라집니다. 이 때 `useCallback`을 사용하면 이전에 생성된 함수의 참조값을 재사용할 수 있습니다.

## ❓부모 컴포넌트에서 React.memo를 적용한 자식 컴포넌트에 useCallback을 적용하지 않은 함수를 넘겨주면 리렌더링을 유발하게 되나요?

네, 맞습니다. 부모 컴포넌트가 리렌더링 되면 함수가 재생성되고 이전과 다른 참조값을 갖게 되는데, 자식 컴포넌트가 prop으로 받는 함수의 참조가 달라졌기 때문에 얕은 비교를 하는 `React.memo`는 자식 컴포넌트를 리렌더링 시킵니다.  
[예시 코드](https://stackblitz.com/edit/stackblitz-starters-4dmbem?devToolsHeight=33&file=src%2FApp.tsx)

## ❓props와 state에 대해 설명해 주세요.

props는 부모 컴포넌트가 자식 컴포넌트로 내려주는 데이터입니다. 반면 state는 컴포넌트 내부적으로 관리하는 동적인 데이터입니다. 리액트에서 state가 변경되면 해당 컴포넌트가 다시 렌더링 됩니다.

## ❓props drilling이란 무엇인가요?

리액트는 데이터를 전달할 때 부모 컴포넌트에서 자식 컴포넌트로 props를 통해 단방향으로 전달합니다.

이러한 특성으로 인해 컴포넌트 계층이 깊어지게 되면 상위 컴포넌트에서 하위 컴포넌트로 props를 전달하기 위해 중간에 props를 받고 넘기기만 하는 컴포넌트가 생길 수 있습니다. 이러한 현상을 props drilling 이라고 합니다.

## ❓props drilling을 해결하는 방법은 무엇인가요?

리액트에서 자체적으로 제공하는 context를 이용하여 전역으로 상태관리를 할 수 있습니다. 혹은 redux, recoil과 같은 서드파티 상태관리 라이브러리를 이용할 수도 있습니다.

## ❓zustand를 선택한 이유와 zustand의 특징에 대해 설명해 주세요.

- Zustand의 장점은 다음과 같습니다.

  - Context API에 비해 보일러 플레이트가 적기 때문에 사용하기 편리합니다.

  - Redux Devtools를 사용할 수 있어 디버깅에 용이합니다.

  - Provider가 필요 없고, 이때문에 불필요한 리렌더링을 최소화시킬 수 있습니다.

- Zustand의 특징은 다음과 같습니다.

  - MVC 패턴이 아닌 Flux 패턴을 이용합니다.

### MVC

Model, View, Controller의 약자로, Model에 데이터를 저장하고 Controller를 이용하여 Model의 데이터를 관리, Model의 데이터가 변경되면 View로 전달하여 사용자에게 보여지는 아키텍처를 말합니다. 사용자가 View를 통해 데이터를 입력하면 View 역시 Model을 업데이트할 수 있으며, 데이터가 양방향으로 흐를 수 있다는 특징을 가지고 있습니다.

### Flux

사용자 입력을 기반으로 Action을 만들고 Action을 Dispatcher에 전달하여 Store의 데이터를 변경한 뒤 View에 반영하는 단방향의 흐름으로 애플리케이션을 만드는 아키텍처를 말합니다.

## ❓Stale-While-Revalidate가 무엇인지 설명해주세요.

브라우저는 `Cache-Control` 의 `max-age` 를 기준으로 캐싱된 컨텐츠를 재사용할지 여부를 판단하게 됩니다. 이때 `stale-while-revalidate` 라는 항목을 추가로 사용함으로써 캐싱 전략을 확장할 수 있습니다.

```JSON
Cache-Control: max-age=1, stale-while-revalidate=59
```

만약 데이터가 이러한 헤더를 가지고 있다면, 브라우저는 아래와 같이 동작합니다.

1. HTTP 요청이 1초(~max-age) 이내에 반복적으로 발생했을 때
   유효성 검증 없이 캐싱된 컨텐츠를 반환합니다.

2. HTTP 요청이 1 ~ 60초(max-age ~ swr) 사이에 반복적으로 발생했을 때
   우선 캐싱된 컨텐츠를 반환하고, 이와 동시에 캐싱된 컨텐츠를 새로운 컨텐츠로 채우도록 서버에 요청합니다.

3. HTTP 요청이 60초(swr~) 이후에 발생했을 때
   요청이 서버로 바로 전달되어 새로운 컨텐츠를 반환받습니다.

## ❓virtual DOM에 대해 설명해 주세요.

Virtual DOM은 메모리상에 존재하는 가상의 DOM입니다. 일반적으로 브라우저에서 DOM의 변경이 발생하면, 브라우저는 해당 변경을 적용하고, 리플로우와 리페인트 작업을 수행합니다. 이런 방식은 DOM의 변경이 많을 때 렌더링 성능에 부정적인 영향을 미치며, 대규모 또는 복잡한 애플리케이션에서는 성능 문제가 발생할 수 있습니다.

React는 실제 DOM의 복사본인 virtual DOM 을 사용해서 상태가 변경될 때마다 Virtual DOM을 다시 생성하고 이전과 새로운 Virtual DOM을 비교하여 변경된 부분을 실제 DOM에 한번에 적용합니다. 이렇게 하면 리플로우 및 리페인트 작업의 횟수를 획기적으로 줄여서 성능을 크게 향상시킬 수 있습니다.

## ❓virtual DOM의 구조는 어떻게 이루어져 있나요?

current, workInProgress 두개의 트리가 존재합니다. current는 마운트가 끝난 트리이고, workInProgress는 업데이트가 적용 중인 트리입니다. 이러한 구조를 더블 버퍼링 이라고 하며, workInProgress 트리는 Commit Phase를 지나게 되면 Current 트리로 변경이되고, 새로운 workInProgress 트리가 생성되는 과정이 반복됩니다.

## ❓DOM과 virtual DOM의 차이점에 대해 설명해 주세요.

DOM은 현재 화면에 현재 렌더링 된 트리이며 DOM에 직접 접근할 수 있는 API를 제공합니다. 하지만 Virtual DOM은 DOM의 복사본이며 DOM에 접근할 수 없습니다. 또한 virtual DOM 자체를 직접 수정할 수도 없으며 state의 변경에 의해서만 가능합니다.

## ❓virtual DOM의 사용으로 성능이 저하될 수 있는 경우엔 무엇이 있나요?

SPA에 사용자의 인터랙션이 많고, 그에 따라 데이터가 자주 변경되는 웹페이지라면 Virtual DOM을 통해 업데이트 연산 횟수를 줄임으로써 성능을 향상시킬 수 있습니다.

하지만 React는 실제 DOM 연산에 Virtual DOM 연산이 합쳐지는 것이기 때문에, 만약 사용자의 인터랙션이 발생하지 않는 정적인 웹페이지를 렌더링할 때 Virtual DOM을 사용하는 것은 성능이 좋지 않을 수 있습니다.

## ❓Hydration이 무엇인가요?

Hydration은 서버에서 렌더링 된 HTML에 이벤트 리스너 또는 상태를 부여해서 유저와 상호작용 할 수 있도록 만드는 과정입니다.

## ❓웹페이지의 성능 지표 중 Hydration과 연관될 수 있는 것은 무엇이 있을까요?

Hydration을 한다는 것은 SSR을 한다는 뜻이므로 SSR과 연관된 성능 지표들을 꼽을 수 있습니다.

- TTV(Time To View): 빠름
- FCP(First Contentful Paint): 빠름
- LCP(Largest Content Paint): 빠름
- TTI(Time To Interact): 느림
- FID(First Input Delay): 느림

## ❓Hydration을 최적화할 수 있는 방안은 무엇이 있을까요?

- Progressive Hydration
- Selective Hydration

## ❓ setState는 비동기인가요?

setState의 역할은 컴포넌트의 state 객체를 업데이트하는 것이다. 리액트에서는 state가 변경되면 컴포넌트가 리렌더링된다.

setState 함수는 동기 함수이지만 setState 함수 호출은 비동기적으로 일어난다.
리액트의 setState가 동기 함수인데 마치 비동기 함수처럼 보이는 이유는 리액트의 리렌더링 원리가 가상 돔을 통해 비동기적으로 작동하기 때문이다.

## ❓ Batch Update는?

React에서 배치 업데이트(Batch Update)란
컴포넌트가 여러 번 업데이트 되더라도, 실제 DOM 요소에 변경사항을 적용하는 것을 최소화하여 애플리케이션의 성능을 최적화하는 기능

## ❓ setState(num + 1) vs setState(num ⇒ num + 1)?

setState(num + 1): 이 방법은 현재 상태 값을 가져와서 그에 1을 더한 값을 설정합니다. 이는 상태가 비동기적으로 업데이트되는 경우 문제를 일으킬 수 있습니다. 여러 번 호출되면 예상치 못한 동작이 발생할 수 있습니다.

setState(num => num + 1): 이 방법은 이전 상태 값을 받아와서 함수를 통해 새로운 상태 값을 계산합니다. 이전 상태를 정확히 알 수 있기 때문에 비동기적 업데이트에서 발생하는 문제를 방지할 수 있습니다.

## ❓ state는 변경되는 값이고, const는 선언된 변수는 값이 변경되면 안되는데, 어떻게 된걸까?

state는 일반 변수와 달리 setState 함수를 이용하여 값을 변경한다. (이렇게 함수 내부의 변수가 함수 수명이 끝나더라도, 변수의 참조가 계속된다면 그 변수의 수명은 계속된다. 이것을 클로저라고 한다. 이렇게 우린 컴포넌트가 render가 되더라도 state를 기억할 수 있다.

허나, let을 사용하게되면 counter2 = 100과 같은 변수 형식의 할당이 가능해진다. 따라서 이를 방지하고 setState를 사용한 변수 변경만 허락하기위해 const로 선언한다. (const로 선언하면 변수 형태의 재할당을 막을 수 있다.)

## ❓리액트 라이프 사이클은 무엇인가요

모든 리액트 컴포넌트에는 라이프사이클이 존재. 컴포넌트의 수명은 페이지에 렌더링되기 전인 준비과정에서 시작하여 페이지에 사라질때 끝난다.
리액트 라이프사이클은 총세가지 카테고리를 가진다. 마운트, 업데이트, 언마운트

**마운트**  
DOM이 생성되고 웹 브라우저상에 나타나는 것을 마운트(mount)

**업데이트**

1. props가 바뀔 때

2. state가 바뀔 때

3. 부모 컴포넌트가 리렌더링될 때

4. this.forceUpdate로 강제로 렌더링을 트리거할 때

**언마운트**  
마운트의 반대 과정, 즉 컴포넌트를 DOM에서 제거하는 것을 언마운트(unmount)

## ❓ 렌더링과 마운팅의 차이에 대해 말씀해주세요

mount는 리액트가 처음으로 구성요소를 렌더링하고 실제로 초기 DOM을 빌드하는 것입니다. render는 DOM생성을 위해 함수가 호출될 때(혹은 클래스 기반 메서드가 호출될 때)입니다

## ❓ 코드 mount, unmount, update

```
useEffect(() => {}, [])
useEffect(() => {}, [a, b])
useEffect(() => {return () => {}}
```

해당 코드를 mount, unmount, update 관점에서 설명해주세요.

```
useEffect(() => {}, []) // ComponetDidMount
useEffect(() => {}, [a, b]) // ComponentDidUpdate
useEffect(() => {return () => {}}) // ComponentWillUnmount 컴포넌트가 언마운트 되기 직전에 실행됨
```

## ❓ React에서 key를 사용해야 하는 이유는 무엇이고, 어떤 데이터를 key로 사용해야 하나요?

리액트에서 배열 데이터 렌더링의 경우 자식들에 key 속성을 부여한다. 주로 key 값으로는 데이터의 고유한 id 값을 사용하고 이는 전역이 아닌 형제 요소에서 유일하면 된다.

또한 배열 데이터 렌더링 이외에도 컴포넌트에 key 속성을 주어 활용할 수 있다. 예를 들어 초기화 되어야 하는 상태를 처리할 때, key 값을 줘서 현재 컴포넌트를 업데이트 하는 것이 아닌 새로운 컴포넌트의 인스턴스를 생성하여 컴포넌트가 초기화된 상태를 가질 수 있도록 한다. 이것은 key를 식별자로 처리하여 이전의 DOMTree의 element를 삭제하고 새로 재생성하도록 하는 원리를 이용하는 것이다.

또한, key 값은 형제간 고유해야 하고, 변경 되어서 안되다. 렌더링 동안 생성되어서는 안된다는 것이다.

### key값으로 인덱스를 사용할 경우 성능상의 불리한점

key 값으로 index를 사용할 경우 배열의 순서가 재배열 되는 경우, VDOM에서 key 값이 식별자의 역할을 하지 못해 DOM Tree를 삭제하고 새로 렌더링 된다. 이는 불필요한 렌더링을 발생시켜 성능상의 문제로 이어진다.

## ❓render와 commit 과정에 대해 설명해 주세요.

Life Cycle에서 컴포넌트 DOM 상태에 따라 마운트, 업데이트 언마운트(class 컴포넌트 경우)로 나뉘고 여기서 Render Phase와 Commit Phase로 단계가 나뉘어 있다.

**Render Phase**
VDOM을 재조정(reconciliation)하는 단계이다.

### 재조정(reconciliation)이란?

VDOM과 실제 DOM 사이의 변경 사항을 처리하고 화면을 업데이트 하는 과정을 말한다. VDOM은 fiber node로 구성된 Tree 형태로 current Tree, workInProgress Tree를 가진 두 개의 트리를 가진 **더블 버퍼링 구조** 이다.

current Tree
DOM에 mount된 정보들을 Fiber node로 표현한 것들로 구성된 Tree를 current Tree라 한다. current Tree는 ReactDOM.render에 인자로 <App />과 연결된 Root Node와 연결되어 있다. HTML element를 가지고 와서 Root를 만든다. 즉, current Tree는 실제 HTML 태그에 적용이 되어 있는 정보를 담고 있는 fiber node로 구성된 Tree이다.

workInProgress
workInProgress는 말그대로 작업중이라는 뜻으로 current Tree를 복제해서 만들어진다. 동일하게 fiber node로 구성되어 alternamte를 통해 current와 서로 참조하고 있다. alternate는 fiber node 객체 안에 alternate라는 key를 가지고 있고, key의 value 값으로 reference 참조가 담겨 있다는 의미이다.

render phase에서 작업인 추가, 수정, 삭제가 일어나는 fiber node tree이다.
작업이 끝나고 commit phase를 지나면 workInProgress는 다시 current Tree가 된다.

- Root Node에서 current로 연결되어 있는 것이 끊어지고 workInProgress로 연결된다.
- 그리고 나서 workInProgress는 current가 되고 이를 복제해서 새로운 workInProgress를 다시 만든다.

-> 이렇게 같은 정보를 가지고 있는 두 개의 트리 구조를 더블 버퍼링 구조라 한다.

이는 작업 중인 workInProgress Tree를 삭제하거나 순서를 조정해야 할 경우, 원본이 필요하고 이를 참조해야 하기 때문이다.

**Commit Phase**
재조정이 끝난 VDOM을 DOM에 적용하고 라이프사이클을 실행하는 단계이다.

- 일괄적으로 처리하기 위해 sync적으로 실행된다
- 리액트가 DOM을 다 조작한 다음 콜스택을 다 비워줘야 그 다음 브라우저가 조작이 끝난 DOM을 paint하기 위해 함수를 호출할 수 있다.
- 브라우저가 paint하기 위해서는 DOM 조작이 완료되어야 한다.

### VDOM 구조

Virtual DOM 구조에 대해서 fiber node로 구성된 Tree 형태이고 더블 버퍼링 구조를 가지고 있다. fiber node들이 부모-자식 관계에서 자식 node를 child로 참조하고 있고 부모 node를 return으로 참조하고 있다. 그리고 두 번째 자식은 부모가 참조하고 있지 않다. 부모는 첫 번째 자식만 참조하고 있고 두 번째 이후부터는 첫 번째 자식이 sibling이라는 키 값으로 객체의 reference를 참조하고 있다.

렌더링 : 컴포넌트로 호출한 다음 react element를 return 받았을때 Fiber node로 확장이 되어지고 fiber node가 VDOM에 반영되는 것까지를 렌더링이라 한다.

그 이후 VDOM에 반영이 된 다음에, DOM에 mount되고 mount 된 DOM을 브라우저가 paint하는 과정으로 이어진다.
