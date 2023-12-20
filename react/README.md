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

## ❓ Strict mode에 대해 설명해 주세요.

`<StrictMode>`는 개발 중에 컴포넌트에서 일반적인 버그를 빠르게 찾을 수 있도록 합니다.

### 컴포넌트 이중 렌더링

순수하지 않은 렌더링으로 인해 발생하는 버그를 찾기 위해 컴포넌트가 추가로 다시 렌더링됩니다. React는 작성하는 모든 컴포넌트가 순수 함수라고 가정합니다. 이것은 React 컴포넌트는 항상 동일한 입력(props, state, context)에 대해 동일한 JSX를 반환해야 한다는 것을 의미합니다.

이 규칙을 위반하는 컴포넌트는 예기치 않게 동작하며 버그를 일으킵니다. `Strict Mode`는 실수로 작성된 순수하지 않은 코드를 찾아내기 위해 (순수 함수여야 하는) 몇 가지 함수를 `개발 환경에서 두 번 호출`합니다.

- 컴포넌트 함수 본문 (이벤트 핸들러 내부 코드를 포함하지 않도록 최상위 로직에서만)
- `useState`, `set` 함수, `useMemo`, `useReducer`에 전달한 함수
- `constructor`, `render`, `shouldComponentUpdate`와 같은 일부 클래스 컴포넌트 메소드

함수가 순수한 경우 두 번 실행하여도 동작이 변경되지 않습니다. 순수 함수는 항상 같은 결과를 생성하기 때문입니다. 그러나 함수가 순수하지 않다면 (예: 받은 데이터를 변경하는 함수) 두 번 실행 시 보통 알아챌 수 있습니다. (이것이 순수하지 않은 이유입니다!) 이는 버그를 조기에 발견하고 수정하는 데 도움이 됩니다.

### Effect 다시 실행

Effect 클린업이 누락되어 발생하는 버그를 찾기 위해 컴포넌트가 추가로 Effect를 다시 실행합니다. 모든 Effect에는 몇 가지 셋업 코드가 있고 어쩌면 클린업 코드가 있을 수 있습니다. 일반적으로 React는 컴포넌트가 `마운트`(화면에 추가)될 때 셋업을 호출하고 컴포넌트가 `언마운트`(화면에서 제거)될 때 클린업을 호출합니다. 그런 다음 React는 마지막 렌더링 이후로부터 의존성이 변경된 경우 클린업과 셋업을 다시 호출합니다. 개발 환경에서 Effect에 대해 `셋업` 대신 `셋업 -> 클린업 -> 셋업`을 실행하면 `Strict Mode`에서 누락된 클린업 로직이 더 눈에 띄게 됩니다.

`Strict Mode`가 켜져 있으면 React는 `모든 Effect에 대해 개발 환경에서 한 번 더 셋업 + 클린업 사이클을 실행`합니다. 의외로 느껴질 수도 있지만 수동으로 파악하기 어려운 미묘한 버그를 드러내는 데 도움이 됩니다.

### 컴포넌트 검사

더 이상 사용되지 않는 API의 사용 여부를 확인하기 위해 컴포넌트를 검사합니다. React는 `<StrictMode>` 트리 내부의 컴포넌트가 더 이상 사용되지 않는 다음 API 중 하나를 사용하는 경우 경고를 표시합니다.

- `findDOMNode`
- `UNSAFE_componentWillMount`와 같은 `UNSAFE_` 클래스 생명주기 메서드
- 레거시 context (`childContextTypes`, `contextTypes`, `getChildContext`)
- 레거시 문자열 refs (`this.refs`)

## ❓tanstack-query/react의 v5에서 바뀐 점에 대해 설명해 주세요.

`useQuery`에서 `onSuccess`, `onError`, `onSettled` 콜백이 제거되었습니다.

콜백들을 제거한 가장 큰 이유는 아래와 같습니다.

1. 콜백이 중복으로 호출될 수 있는 위험
2. 상태 동기화를 목적으로 사용했을 때 발생하는 추가 렌더링 사이클 및 잘못된 데이터 노출의 가능성. 예) `onSuccess` 콜백에서 지역 또는 전역 상태 업데이트
3. 콜백이 실행되지 않을 수 있는 위험성 예) `staleTime` 설정으로 `query function`이 호출되지 않아 `onSuccess` 콜백이 실행되지 않는 경우

v5부터는 콜백을 다음과 같은 방법을 제시합니다.

1. 전역 캐시 레벨에서 콜백으로 처리
2. Error Boundary로 에러 처리
3. status enum, isError 등으로 컴포넌트 내에서 처리

### staleTime, gcTime에 따라 쿼리의 상태가 변경이 되는데, 이 상태들에 대해 설명해 주세요.

<image src="../images/query-data-status.png" />

`gcTime`은 기본값인 5분, `staleTime`은 기본값인 `0`을 사용한다고 가정해 보겠습니다.

- `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })`의 새 인스턴스가 마운트 됩니다.
  - 쿼리 키 `['todos']`로 다른 쿼리가 수행되지 않았으므로 이 쿼리는 하드 로딩 상태를 표시하고 데이터를 가져오기 위한 네트워크 요청을 수행합니다.
  - 네트워크 요청이 완료되면 반환된 데이터는 쿼리 키 `['todos']`로 캐싱됩니다.
  - 훅은 설정된 `staleTime`(기본값 `0` 또는 즉시)이 지나면 데이터를 `stale`(오래된 것)으로 표시합니다.
- `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })`의 두 번째 인스턴스가 다른 곳에서 마운트됩니다.
  - 캐시의 쿼리 키 `['todos']`에 첫 번째 쿼리가 가져온 데이터가 이미 있으므로 해당 데이터는 캐시에서 즉시 반환됩니다.
  - 새 인스턴스는 해당 쿼리 함수를 사용하여 새 네트워크 요청을 트리거합니다.
    - 두 쿼리 함수 `fetchTodos`가 동일한지 여부에 관계없이 쿼리 키가 동일하기 때문에 두 쿼리의 `status`는 모두 업데이트됩니다(`isFetching`, `isPending` 및 기타 관련 값 포함).
  - 요청이 성공적으로 완료되면 쿼리 키 `['todos']`의 캐시 데이터가 새 데이터로 업데이트되고 두 인스턴스 모두 새 데이터로 업데이트됩니다.
- `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })`의 두 인스턴스가 모두 언마운트 되어 더 이상 사용되지 않습니다.
  - 이 쿼리의 활성(active) 인스턴스가 더 이상 없으므로 쿼리 삭제 및 가비지 콜렉팅을 수행하기 위해 `gcTime`을 가비지 컬렉팅 시간으로 설정합니다(기본값 `5분`).
- 캐시 만료 시간이 되기 전에 다른 `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })` 인스턴스가 마운트됩니다. 이 쿼리는 백그라운드에서 `fetchTodos` 함수가 실행되는 동안 사용 가능한 캐시 데이터를 즉시 반환합니다. 함수가 성공적으로 완료되면 캐시를 새로운 데이터로 채웁니다.
- `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })`의 마지막 인스턴스가 언마운트 됩니다.
- 5분 이내에 `useQuery({ queryKey: ['todos'], queryFn: fetchTodos })`의 인스턴스가 더 이상 나타나지 않습니다.
  - `['todos]` 키로 캐시된 데이터가 삭제되고 가비지 콜렉팅 됩니다.
