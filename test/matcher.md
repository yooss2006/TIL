# matcher

## toBe

```jsx
const fn = require("./fn");

test("설명", () => {
  expect(1).toBe(1);
});

test("2더하기 3은 5야", () => {
  expect(fn.add(2, 3)).toBe(5);
});

test("2더하기 5는 6이 아니야", () => {
  expect(fn.add(2, 5)).not.toBe(6);
});
```

여기서 `tobe()`를 `matcher`라고 한다.

- 숫자나 문자등 기본 파일 값을 비교할 때 사용한다.
- toBe 앞에 `.not`을 붙이면 결과가 아닌 지를 검사한다.

<br/>

## toEqual, toStrictEqual

```jsx
const fn = {
  add: (num1, num2) => num1 + num2,
  makeUser: (name, age) => ({ name, age }),
};

module.exports = fn;
```

위 처럼 객체를 반환하는 함수의 경우 `toEqual`, `toStrictEqual`을 사용한다.

- 배열의 경우도 마찬가지

```jsx
test("이름과 나이를 입력받아 객체를 반환해줘", () => {
  expect(fn.makeUser("yoo", 26)).toEqual({ name: "yoo", age: 26 });
});
test("이름과 나이를 입력받아 객체를 반환해줘", () => {
  expect(fn.makeUser("yoo", 26)).toStrictEqual({ name: "yoo", age: 26 });
});
```

둘의 차이는 `toStrictEqual`이 더 엄격한 테스트로 객체안에 `undefind`가 들어가 있는 경우 에러를 발생시킨다.

<br/>

## toBeNull, toBeUndefined, toBeDefined

```jsx
test("null은 null입니다.", () => {
  expect(null).toBeNull();
});
```

위처럼 `null`, `undefined`, `defined`인 경우 통과된다.

<br/>

## toBeTruthy, toBeFalsy

```jsx
test("0은 false 입니다.", () => {
  expect(0).toBeFalsy();
});
```

값이 참인지 거짓인지를 테스트한다.

<br/>

## 크기 비교

- **toBeGreaterThan** 크다.
- **toBeGreaterThanOrEqual** 크거나 같다.
- **toBeLessThan** 작다
- **toBeLessThanOrEqual** 작거나 같다.
- 정확히 몇 글자와 같다라면 **toBe**를 사용하자.

```jsx
test("ID는 10글자 이하여야 합니다.", () => {
  const id = "yoofh2006ddd";
  expect(id.length).toBeLessThan(15);
});
```

0.1+0.2와 같은 소수의 연산은 소수점을 정확하게 계산하지 못한다.

그 이유는 2진수로 계산하기 때문이다.

이럴 땐 `toBeCloseTo`를 사용하자.

```jsx
test("0.1+0.2는 0.3이다.", () => {
  expect(fn.add(0.1, 0.2)).toBeCloseTo(0.3);
});
```

<br/>

## toMatch

어떤 문자열에 어떤 글자가 있는지 테스트할 수 있다.

- 정규표현식을 사용할 수 있다.

```jsx
test("Hello World에 a라는 글자가 있나?", () => {
  expect("Hello World").toMatch(/H/);
});
```

<br/>

## toContain

배열안에 어떤 요소가 있는지 테스트할 수 있다.

```jsx
test("유저리스트에 mike가 있나?", () => {
  const user = "Mike";
  const userList = ["kim", "Mike"];
  expect(userList).toContain(user);
});
```

<br/>

## toThrow

예외가 발생했는지를 테스트한다.

```jsx
const fn = {
  throwErr: () => {
    throw new Error("xx");
  },
};

module.exports = fn;
```

에러를 만드는 메서드를 만들었다.

```jsx
test("이거 에러나나요?", () => {
  expect(() => throwErr()).toThrow();
});
```

어떤 예외인지도 테스트할 수 있다.

```jsx
test("이거 에러나나요?", () => {
  expect(() => throwErr()).toThrow("xx");
});
```
