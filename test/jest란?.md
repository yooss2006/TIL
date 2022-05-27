# jest란?

페이스북이 만든 테스팅 도구

**장점** : 별도의 설정없이 빠르게 테스트 케이스를 작성할 수 있다.

Html 요소를 탐색하는데 **aria(접근성 마커)를 사용한다. 이는** TDD 를 작성함으로서 자연스럽게 접근성을 향상시킬 수 있다.

## 설치 방법

```jsx
npm init
npm install jest -—save-dev
```

`package.json` 파일에서 test 부분을 지우고 `“jest”`를 적어준다.

## 사용방법

`fn.js`에 object 형태의 메서드로 함수를 만든다.

```jsx
const fn = {
  add: (num1, num2) => num1 + num2,
};
module.exports = fn;
```

`fn.test.js` 라는 같은 이름에 `.test`가 붙은 파일을 만들어준다.

- `.test.js`로 끝나는 경우
- `__tests__`라는 이름인 경우

테스트 파일로 인식해 전부 테스트한다.

`npm test`로 테스트를 할 수 있다.

테스트 코드

```jsx
test('테스트의 설명', () => {
  expect(테스트 대상).toBe(테스트 결과);
})
```

테스트 코드는 다음의 형태를 띄고 있다.

Test 함수 : 글로벌 함수로 두가지 인자를 가진다.

1. 문자열이 들어가는 테스트의 설명
   - `'테스트의 설명’`
2. 테스트를 실행하는 테스트 함수

   - `expect(테스트 대상).toBe(테스트 결과);`

   `expect()` : 기대한 결과가 성공인지 실패인지를 판단하는 함수
   `toBe(테스트 결과)` : matcher 함수
