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
    level: "GOLD" | "SILVER" | "BRONZE";
    active: boolean;
    createdAt: Date;
    image?: string | undefined;
    ips?: string[] | undefined;
    }

    function updateAccount(account: Account) {
    if (typeof account.id !== "string" || account.id.length < 36)
        throw new Error("Invalid id");
    if (typeof account.age !== "number" || account.age < 0)
        throw new Error("Invalid age");
    if (!["GOLD", "SILVER", "BRONZE"].includes(account.level))
        throw new Error("Invalid level");
    if (!(account.createdAt instanceof Date))
        throw new Error("Invalid createdAt");
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
        level: z.enum(["GOLD", "SILVER", "BRONZE"]),
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
