# TypeScript interface & Generic

## Type vs interface

![Untitled](TypeScript%20interface%20&%20Generic%20e589f4466a1841df8a1ca4bb4a1f80c7/Untitled.png)

type과 interface는 자주 사용하는 타입들을 object 형태의 묶음으로 정의해 

**새로운 타입**을 만드는 기능이다.

---

### 확장(상속)하는 법

### interface

`extends` 키워드를 이용해서 확장할 수 있습니다.

```tsx
interface Person {
  name: string;
  age: number;
}

interface Student extends Person { // 확장(상속)
  school: string;
}

const jieun: Student = {
  name: 'jieun',
  age: 27,
  school: 'HY'
}
```

### type

`&` 기호를 이용해서 확장할 수 있습니다.

```tsx
type Person = {
  name: string,
  age: number
}

type Student = Person & { // 확장(상속)
  school: string
}

const jieun: Student = {
  name: 'jieun',
  age: 27,
  school: 'HY'
}
```

---

### 선언적 확장

### interface

**선언적 확장**(Declaration Merging)이 가능합니다.

(같은 이름의 interface를 선언하면, 자동으로 확장됩니다.)

```tsx
interface Person {
  name: string;
  age: number;
}

interface Person { // 선언적 확장
  gender: string;
}

const jieun: Person = {
  name: 'jieun',
  age: 27,
  gender: 'female'
}
```

### type

선언적 확장이 불가능합니다.

```tsx
type Person = {
  name: string;
  age: number;
}

type Person = { // ❗️Error: Duplicate identifier 'Person'.
  gender: string;
}
```

> ➡️ 따라서 타입 객체의 확장성을 위해서는 interface를 사용하는 것이 더 좋습니다.
> 

## 자료형(type)

### interface

**객체(Object)**의 타입을 설정할 때 사용할 수 있으며, 원시 자료형에는 사용할 수 없다.

```tsx
interface Person {
  name: string;
  age: number;
  gender: string;
}
```

```tsx
interface name extends string { // ❌ 불가능
  ...
}
```

### type

객체 타입을 정의할 때도 사용할 수 있지만, 

객체 타입을 정의할 때는 `interface`를 사용하는게 좋고,

단순한 **원시값(Primitive Type)**이나 **튜플(Tuple)**, **유니언(Union)** 타입을 선언할 때 

`type`을 사용하는 것이 좋습니다.

```tsx
type Name = string; // primitive
type Age = number;
type Person = [string, number, boolean]; // tuple
type NumberString = string | number; // union
```

```tsx
type Person = { // 객체는 interface를 사용하도록 하자.
  name: string,
  age: number,
  gender: string
}
```

## Conclusion

공식 문서에서도 뭘 사용할지 모를 때는 interface를 사용하고

**type의 특성이 필요할 때만 type을 사용하라고 하고 있습니다.**

튜플이나 단순한 원시값의 타입 선언의 경우에는 type을 사용하여야 합니다.

---

## 제네릭(Generics)이란?

![Untitled](TypeScript%20interface%20&%20Generic%20e589f4466a1841df8a1ca4bb4a1f80c7/Untitled%201.png)

제네릭은 C#, Java 등의 언어에서 재사용성이 높은 컴포넌트를 만들 때

자주 활용되는 특징입니다. 

특히, 한 가지 타입보다 **여러 가지 타입에서 동작하는 컴포넌트를 생성하는데 사용됩니다.**

**제네릭이란, 타입을 마치 함수의 파라미터처럼 사용하는 것을 의미합니다**

```jsx
function getText<T>(text: T): T {
  return text;
}
```

위 함수는 제네릭 기본 문법이 적용된 형태입니다. 

타입대신 `T`라는 문자로 지정해 `제네릭 타입`으로 정의합니다. 

이때 문자 `T`를 **타입 변수**라고 합니다.

이제 함수를 호출할 때 아래와 같이 함수 안에서 사용할 타입을 넘겨줄 수 있습니다.

```jsx
getText<string>('hi');
getText<number>(10);
getText<boolean>(true);
```

### 제네릭 사용하는 이유

```jsx
function logText(text: string): string {
  return text;
}
```

이 함수의 인자와 반환 값은 모두 string 으로 지정되어 있지만 

만약 여러 가지 타입을 허용하고 싶다면 아래와 같이 **any**를 사용할 수 있습니다

```jsx
function logText(text: any): any {
  return text;
}
```

이렇게 타입을 바꾼다고 해서 함수의 동작에 문제가 생기진 않습니다. 

다만, 함수의 인자로 어떤 타입이 들어갔고 어떤 값이 반환 되는지는 알 수가 없습니다. 

왜냐하면 **any 타입은 타입 검사를 하지 않기 때문입니다.**

이러한 문제점을 해결할 수 있는 것이 제네릭입니다.

**제네릭을 사용한 경우,**

타입을 나중에 정의할 수 있으므로, 

사용자는 **필요에 따라 유연한 타입을 지정할 수 있습니다.** 

제네릭을 사용하면 타입 안정성을 유지할 수 있습니다.

```jsx
function logText<T>(text: T): T {
  return text;
}
```

**타입 추론도 유리해집니다.**

- **제네릭이 없는 경우:** 함수에 **`any`**를 사용하면 타입 추론이 제대로 이루어지지 않습니다. 타입 정보가 사라지기 때문에 함수를 호출할 때 타입을 명시적으로 지정해야 합니다.
    
    ```jsx
    const result = logText("Hello"); // result의 타입이 any로 추론됨
    ```
    
- **제네릭을 사용한 경우:** 함수를 호출할 때 인자의 타입을 지정하지 않아도 타입 추론이 가능합니다.
    
    ```jsx
    const result = logText("Hello"); // result의 타입이 string으로 추론됨
    ```
    

**코드 재사용이 유리해집니다.**

- **제네릭이 없는 경우:** 타입이 고정된 함수를 여러 번 작성해야 합니다. 예를 들어, **`logText`** 함수를 다른 타입에 대해 사용하려면 새로운 함수를 만들어야 합니다.
    
    ```jsx
    function logNumber(num: number): number {
      return num;
    }
    ```
    
- **제네릭을 사용한 경우:** 하나의 제네릭 함수로 여러 타입에 대해 재사용할 수 있습니다.
    
    ```jsx
    function logValue<T>(value: T): T {
      return value;
    }
    
    const result1 = logValue("Hello");
    const result2 = logValue(42);
    ```
    

---

참고 링크

[https://velog.io/@wlwl99/TypeScript-type과-interface의-차이](https://velog.io/@wlwl99/TypeScript-type%EA%B3%BC-interface%EC%9D%98-%EC%B0%A8%EC%9D%B4)

[https://dkrnfls.tistory.com/277](https://dkrnfls.tistory.com/277)

[https://velog.io/@cyd5538/타입스크립트-제네릭](https://velog.io/@cyd5538/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%A0%9C%EB%84%A4%EB%A6%AD)

[https://velog.io/@bami/Typescript-제네릭-타입](https://velog.io/@bami/Typescript-%EC%A0%9C%EB%84%A4%EB%A6%AD-%ED%83%80%EC%9E%85)
