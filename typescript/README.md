# TypeScript

## ❓타입스크립트에서 unknown, void, never, any에 대해 설명해 주세요

### any와 unknown

#### any타입

- any 타입은 어떠한 타입 검사도 받지 않기 때문에 아무 타입의 값이나 범용적으로 담아 사용할 수 있고 또 다양한 타입의 메서드도 마음대로 호출해서 사용해도 문제가 되지 않습니다.
- 또 any 타입의 값은 어떤 타입으로 정의된 변수던 문제 없이 다 할당할 수 있습니다.

#### unknown 타입

- unknown 타입은 any 타입과 비슷하지만 보다 안전한 타입입니다.
- unknown 타입은 독특하게도 변수의 타입으로 정의되면 모든 값을 할당받을 수 있게 되지만, 반대로 unknown 타입의 값은 그 어떤 타입의 변수에도 할당할 수 없고, 모든 연산에 참가할 수 없게 됩니다. 쉽게 정리하면 오직 값을 저장하는 행위밖에 할 수 없게 됩니다.

### void와 never

#### void 타입

- void 타입은 아무런 값도 없음을 의미하는 타입입니다.
- 아무런 값도 반환하지 않는 함수의 반환값 타입
- void 타입의 변수에는 undefiend 이외의 다른 타입의 값은 담을 수 없습니다. 그 이유는 void 타입이 undefiend 타입을 포함하는 타입이기 때문
- 그런데 만약 이때 tsconfig.json에 엄격한 null 검사(strictNullChecks) 옵션을 해제(False)로 설정하면 특별히 이때에는 void 타입의 변수에 null 값도 담을 수 있게 됩니다.

#### never 타입

- never 타입은 불가능을 의미하는 타입입니다.
- 불가능 한 값의 타입을 정의할 때 never 타입을 사용합니다. ex) 무한루프,의도적으로 오류를 발생시키는 함수

## ❓덕 타이핑에 대해 설명해 주세요.

- 타입을 미리 정하는 것이 아니라, 실행 되었을 때 확인하여 타입을 정하는 것을 말합니다.
- 객체 자체가 어떤 타입인지는 중요하지 않고, 특정 메소드나 속성의 존재로 타입을 판단합니다.

### 장점

- 타입에 대해 매우 자유롭고 빠르게 코드를 작성할 수 있습니다.

### 단점

- 런타임 과정에서 예상치 못한 타입으로 인해 오류가 발생할 수 있습니다.

### 타입스크립트 사용 시 런타임 에러

- 타입스크립트 코드에서 타입 에러는 컴파일 과정에서만 발생합니다.
- 때문에 외부 API에 의존하는 코드일 경우, 이러한 데이터의 유효성을 검증하는 것이 중요합니다.

  ```ts
  interface Account {
    id: string;
    email: string;
    age: number;
    level: 'GOLD' | 'SILVER' | 'BRONZE';
    active: boolean;
    createdAt: Date;
    image?: string | undefined;
    ips?: string[] | undefined;
  }

  function updateAccount(account: Account) {
    if (typeof account.id !== 'string' || account.id.length < 36)
      throw new Error('Invalid id');
    if (typeof account.age !== 'number' || account.age < 0)
      throw new Error('Invalid age');
    if (!['GOLD', 'SILVER', 'BRONZE'].includes(account.level))
      throw new Error('Invalid level');
    if (!(account.createdAt instanceof Date))
      throw new Error('Invalid createdAt');
  }
  // 출처 : https://www.daleseo.com/zod-why-validation/
  ```

- 이를 구현할 수 있는 라이브러리는 `Joi`, `Yup`, `Zod` 등이 있습니다.
  - 이 라이브러리들은 공통적으로 자바스크립트로 스키마를 정의하고, 이를 이용하여 유효성 검증을 가능하게 해줍니다.
    ```js
    // ✅ 스키마 정의
    const Account = z.object({
      id: z.string().uuid(),
      email: z.string().email(),
      age: z.number().int().min(18).max(80),
      level: z.enum(['GOLD', 'SILVER', 'BRONZE']),
      image: z.string().url().max(200).optional(),
      ips: z.string().ip().array().optional(),
      active: z.boolean().default(false),
      createdAt: z.date().default(new Date()),
    });
    // ✅ 스키마로 부터 타입 추론
    type Account = z.infer<typeof Account>;
    function updateAccount(account: Account) {
      // ✅ 스키마로 유효성 검증
      Account.parse(account);
    }
    ```

### 구조적 타이핑

- 실제 구조와 정의에 의해 타입을 결정합니다.
- 타입스크립트는 구조적 타이핑 시스템을 가지고 있습니다.
- 때문에 다른 타입이더라도 같은 속성을 가지고 있다면 재사용할 수 있습니다.

## ❓Type과 Interface의 차이에 대해 설명해주세요.

일단 type과 interface는 타입스크립트에서 타입을 정의하는 키워드입니다. 둘 다 컴파일 타임에 타입 체크를 수행하고, 런타임에서는 타입 정보를 제거합니다.
interface는 객체의 구조를 정의하는데 사용되며, 이를 통해 클래스나 함수의 파라미터 타입, 반환 타입, 변수 등을 정의할 수 있습니다. interface는 extends 키워드를 통해 타입 확장이 가능하고 다른 interface에서 상속하여 새로운 interface를 만들 수 있습니다.
type은 객체 뿐만 아니라 문자열, 숫자 등의 다른 타입도 정의할 수 있습니다. type을 사용하여 새로운 타입을 만들 때 더 많은 유연서을 제공합니다. type은 union type, intersection type, tuple 등 다양한 타입을 정의할 수 있으며, interface와 달리 타입 별칭(type alias)를 사용할 수 있습니다.

### type과 비교하여 interface의 성능상 이점

- interface는 클래스가 선언된 구조의 타입을 확인하는데 사용할 수 있지만 타입 별칭은 사용할 수 없다.
- 일반적으로 타입스크립트 타입 검사기가 더 빨리 작동한다. interface는 타입 별칭이 하는 것처럼 새로운 객체 리터럴의 동적인 복사 붙여넣기 보다 내부적으로 더 쉽게 캐시할 수 있는 명명된 타입을 선언한다.
- interface는 이름 없는 객체 리터럴의 별칭이 아닌 이름이 있는 객체로 간주되므로 어려운 특이 케이스에서 나타나는 오류 메세지를 더 쉽게 읽을 수 있다.
- type은 중복된 이름의 속성을 정의하면 에러가 발생하지만, interface의 경우 동일한 이름의 interface를 여러 개 선언해도 속성이 자동으로 머지된다.

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = { name: 'John', age: 25 }; // 자동으로 머지됨
```

### 문제

아래 코드에서 interface A의 타입은 어떻게 되나요?

```ts
interface A {
  name: string;
}
interface A {
  name: number;
}
```

interface의 중요한 특징 중 하나는 서로 병합한다는 특징이 있다. 두 개의 interface가 A로 정의되었고 동일한 스코프에 선언된 경우, 선언된 모든 필드를 포함하는 더 큰 interface가 코드에 추가(머지) 된다.
그러나 현재 위 코드는 interface A의 name 속성이 서로 다른 타입으로 선언되어 충돌이 발생했기 때문에 발생합니다. TypeScript는 타입의 일관성을 유지하기 위해 이러한 충돌을 허용하지 않는다.

타입스크립트에서 interface 병합은 외부 패키지 또는 Window 같은 내장된 전역 interface를 보강하는데 유용하다. 그러나 일반적으로 타입스크립트 개발 시 interface의 병합은 자주 사용되지 않는다. interface가 여러 곳에 선언되면 코드를 이해하기 어렵고 가독성이 좋지 않기 때문에 가능하면 사용하지 않는 것이 좋다.
