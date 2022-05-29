# asynchronous

## 콜백함수 테스트

```jsx
const fn = {
  getName: (callback) => {
    const name = "mike";
    setTimeout(() => {
      callback(name);
    }, 3000);
  },
};

module.exports = fn;
```

```jsx
test("3초 후에 받아온 이름은 Mike이다.", () => {
  function callback(name) {
    expect(name).toBe("mike");
  }
  fn.getName(callback);
});
```

위 결과는 성공했지만 3초를 기다리지 않고 실행되며 mike가 아닌 다른 값을 넣어도 성공했다고 나온다.

이를 해결하기 위해선 테스트 함수에 `done`이라는 콜백함수를 전달해줘야 한다.

- `done()`이 호출되기 전까지 테스트를 끝내지 않고 기다린다.

```jsx
test("3초 후에 받아온 이름은 Mike이다.", (done) => {
  function callback(name) {
    expect(name).toBe("mike");
    done();
  }
  fn.getName(callback);
});
```

주의 : `done`을 전달받았지만 호출하지 않으면 테스트는 실패한다.

테스트 함수에서 예외처리를 하고 싶다면 try-catch문으로 감싸면 된다.

```jsx
test("3초 후에 받아온 이름은 Mike이다.", (done) => {
  function callback(name) {
    try {
      expect(name).toBe("mike");
      done();
    } catch (error) {
      done();
    }
  }
  fn.getName(callback);
});
```

</br>

## promise

```jsx
const fn = {
  getAge: () => {
    const age = 30;
    return new Promise((res, rej) => {
      setTimeout(() => {
        res(age);
      }, 3000);
    });
  },
};

module.exports = fn;
```

위와 같은 프로미스를 반환하는 메서드를 만들었다.

promise의 경우 `done`을 전달할 필요가 없다.

```jsx
//promise 비동기 테스트
test("3초 후에 받아온 나이는 30", () => {
  return fn.getAge().then((age) => {
    expect(age).toBe(30);
  });
});
```

대신 `return`을 사용해 반환해줘야 비동기 처리를 사용할 수 있다.

또는 `**resolves**`, `**rejects**`를 이용해 더 간편하게 처리할 수 있다.

```jsx
test("3초 후에 받아온 나이는 30", () => {
  return expect(fn.getAge()).resolves.toBe(30);
});
```

</br>

## async await

`async` `await`을 사용해서 좀 더 쉽게 구현이 가능하다.

```jsx
test("3초 후에 받아온 나이는 30", async () => {
  const age = await fn.getAge();
  expect(age).toBe(30);
});
```

`resolves`, `rejects`에서도 응용이 가능하다.

```jsx
test("3초 후에 받아온 나이는 30", async () => {
  await expect(fn.getAge()).resolves.toBe(30);
});
```
