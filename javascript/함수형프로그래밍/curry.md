# curry

curry는 함수를 받아 함수를 반환하는 함수입니다.

하지만 pipe와 다른 점은 인자를 받아 인자가 원하는 만큼 들어왔을 때 받아둔 함수를 나중에 평가시키는 함수입니다.

1. `curryFunc(3)`처럼 일자를 하나만 받았을 때는 함수를 반환해 실행이 안됨
2. `curryFunc(3,2)`처럼 한 번에 2개는 실행됨
3. `curryFunc(3)(2)` 이런 식으로 인자를 2개를 받아도 실행됨

### curry를 만드는 방법

```jsx
const curry =
  (f) =>
  (a, ..._) =>
    _.length ? f(a, ..._) : (..._) => f(a, ..._);
const mult = curry((a, b) => a * b);
```

쉽게 볼 수 있도록 풀어서 설명하겠습니다.

`const curry = (f) => ()=> {}` : curry는 함수를 받고 반환되는 값도 함수입니다.

`(a, ..._) =>` 반환되는 함수는 두 개의 인자를 받습니다. 이때 이 함수가 반환하는 값이 인자의 개수에 따라 결정됩니다.

`.length ? f(a, ..._) : (..._) => f(a, ..._)`

1.  인자를 두 개 이상 받으면 `f(a, ..._)`가 실행되어 `a*b`의 값이 반환됩니다.
2.  인자를 한 개 이하로 받으면 `(..._) => f(a, ..._)`를 반환하여 실행되지 않습니다.

따라서 위 mult에 `mult()`, `mult(1)`을 넣은 경우 작동하지 않으며 `mult(1,3)` 또는 `mult(1)(3)`을 넣으면 작동하게 됩니다.

```jsx
const mult3 = mult(3);
mult3(2);
```

위처럼 인자를 하나만 받은 경우 `(..._) => f(a, ..._)`를 반환하기에 `mult3(2)`와 같은 형태도 가능해집니다.

### go와 curry를 결합해보기

기존에 구현한 map, filter, reduce 함수에 curry를 적용해 해당 함수를 축약한 go 함수로 표현이 가능해집니다.

```jsx
// map
const map = curry((f, iter) => {
  const res = [];
  for (const el of iter) {
    res.push(f(el));
  }
  return res;
});

// filter
const filter = curry((f, iter) => {
  let res = [];
  for (const a of iter) {
    if (f(a)) res.push(a);
  }
  return res;
});

// reduce
const reduce = curry((f, acc, iter) => {
  if (!iter) {
    iter = acc[Symbol.iterator]();
    acc = iter.next().value;
  }
  for (const a of iter) {
    acc = f(acc, a);
  }
  return acc;
});
```

위처럼 curry를 감싸게 되면 map, filter, reduce 함수는 인자를 2개 이상 받아야지만 동작하는 함수가 됩니다.

```jsx
go(
  products,
  (products) => filter((p) => p.price < 20000, products),
  (products) => map((p) => p.price, products),
  (prices) => reduce(add, prices),
  console.log
);
```

이전에 go를 이용해 함수의 실행 순서대로 보이게 한 위 함수를 다음과 같은 형태로 바꿀 수 있습니다.

```jsx
//curry가 적용된 모습
go(
  products,
  (products) => filter((p) => p.price < 20000)(products),
  (products) => map((p) => p.price)(products),
  (prices) => reduce(add)(prices),
  console.log
);

//curry를 통한 축약이 가능해집니다.
go(
  products,
  filter((p) => p.price < 20000),
  map((p) => p.price),
  reduce(add),
  console.log
);
```
