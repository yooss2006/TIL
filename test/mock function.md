# 목함수

테스트를 하기 위해 흉내만 내는 함수를 의미한다.

<br/>

## 목함수를 만드는 방법

```jsx
const mockFn = jest.fn();
```

<br/>

## 간단한 사용법

```jsx
mockFn();
mockFn(1);
```

테스트를 진행하는데 목함수에 어떤 값이 들었는가 확인해보았다.

```jsx
test("dd", () => {
  console.log(mockFn.mock.calls);
  expect("dd").toBe("dd");
});
```

결과는 `[ [], [ 1 ] ]`으로 목함수가 실행된 횟수와 어떤 인수를 넣었는가를 알 수 있다. 이를 응용해 다음과 같이 사용할 수 있다.

```jsx
test("목함수 호출 횟수는 2번입니다.", () => {
  expect(mockFn.mock.calls.length).toBe(2);
});
test("2번째로 호출된 함수에 전달된 첫번째 인수는 1입니다.", () => {
  expect(mockFn.mock.calls[1][0]).toBe(1);
});
```

콜백함수를 사용하지 않고 목함수를 이용해 볼 수도 있다.

```jsx
function forEachAdd1(arr) {
  arr.forEach((num) => {
    mockFn(num + 1);
  });
}

forEachAdd1([10, 20, 30]);
test("함수 호출은 3번됩니다.", () => {
  expect(mockFn.mock.calls.length).toBe(3);
});
test("전달된 값은 11, 21, 31 입니다.", () => {
  expect(mockFn.mock.calls[0][0]).toBe(11);
  expect(mockFn.mock.calls[1][0]).toBe(21);
  expect(mockFn.mock.calls[2][0]).toBe(31);
});
```

<br/>

## 목함수의 results

`mockFn.mock.results`를 사용하면 목함수에 들어있는 값들을 object 형태로 볼 수있다

```jsx
const mockFn = jest.fn((num) => num + 1);
mockFn(10);
mockFn(20);
mockFn(30);

test("결과", () => {
  console.log(mockFn.mock.results);
	expect(mockFn.mock.results[0].value).toBe(11);
  expect(mockFn.mock.results[1].value).toBe(21);
  expect(mockFn.mock.results[2].value).toBe(31);
});

results: [ //results의 형태
  { type: 'return', value: 11 },
  { type: 'return', value: 21 },
  { type: 'return', value: 31 }
],
```

<br/>

## mockReturnValueOnce

목함수가 값을 반환해주게 할 수 있다.

```jsx
const mockFn = jest.fn();
mockFn
  .mockReturnValueOnce(true)
  .mockReturnValueOnce(false)
  .mockReturnValueOnce(true)
  .mockReturnValueOnce(false)
  .mockReturnValue(true);

const result = [1, 2, 3, 4, 5].filter((num) => mockFn(num));

test("홀수는 1,3,5", () => {
  expect(result).toStrictEqual([1, 3, 5]);
});
```

위 목함수는 차례대로 true, false를 반환하며 마지막에는 `mockReturnValueOnce`가 아닌 `mockReturnValue`를 사용해야 한다.

<br/>

## mockResolvedValue

목함수가 비동기 처리를 할 수 있게 한다.

```jsx
mockFn.mockResolvedValue({ name: "mike" });

test("받아온 이름은 mike", () => {
  mockFn().then((res) => {
    expect(res.name).toBe("mike");
  });
});
```

<br/>

## 모킹 모듈

```jsx
const fn = {
  add: (num1, num2) => num1 + num2,
  createUser: (name) => {
    console.log("실제로 사용자가 생성되었습니다.");
    return {
      name,
    };
  },
};
```

`createUser` 메서드를 실행하면 name에 해당하는 사용자가 생성된다.

하지만 테스트에서 실제 사용자가 생성되면 다시 삭제해야 하는 번거로움이 생긴다.

이때 모킹 모듈을 이용한다.

```jsx
//모킹 모듈 만들기
jest.mock("./fn");

fn.createUser.mockReturnValue({ name: "mike" });

test("유저를 만든다.", () => {
  const user = fn.createUser("mike");
  expect(user.name).toBe("mike");
});
```

실제로 `createUser`가 동작하진 않고 테스트해볼 수 있다.

<br/>

## 목함수의 유용한 메서드

```jsx
const mockFn = jest.fn();
mockFn(10, 20);
mockFn();
mockFn(30, 40);
test("한 번 이상 호출됐다.", () => {
  expect(mockFn).toBeCalled();
});
test("정확히 3번 호출됐다.", () => {
  expect(mockFn).toBeCalledTimes(3);
});
test("10이랑 20을 전달받은 함수가 있다.", () => {
  expect(mockFn).toBeCalledWith(10, 20);
});
test("마지막 함수는 30이랑 40 받았다.", () => {
  expect(mockFn).lastCalledWith(30, 40);
});
```
