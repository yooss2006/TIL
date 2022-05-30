# express server

## Node.js 및 express 초기 셋팅

1. `npm init`
2. `npm i —save express`

express로 서버를 시작하려면 아래와 같은 코드를 기본으로 띄우고 시작해야한다.

```jsx
const express = require("express");
const app = express();
app.listen();
```

`app.listen`에는 인수가 들어간다.

1. 서버를 띄울 포트번호
2. 띄운 후 실행할 코드

```jsx
app.listen(8080, function () {
  console.log("listening on 8080");
});
```

### get 요청해보기

1. 고객에 주소창에 URL을 입력해 이런이런 요청을 해주세요라고 보낸다.
2. 서버는 요청을 확인하고 응답한다.

라우터 처리를 할때 `“/경로"`, `실행할 코드`가 들어간다.

```jsx
app.get("/", function (req, res) {
  res.send("하이요");
});
```

**html 파일 보내기**

```jsx
app.get("/", function (req, res) {
  **res.sendFile**(__dirname + "/index.html");
});
```

### Post 요청해보기

HTML에서 form 태그로 요청을 보내는 경우 사용한다.

- 클라이언트

```jsx
<form action="/add" method="POST"></form>
```

- 서버

```jsx
app.post("/add", function(req, res) {
	res.send("전송완료");
}
```

전송한 데이터를 사용하려면 `body-parser`라는 라이브러리가 필요하다.

- `body-parser`는 `요청(res)`데이터의 해석을 도와준다.

> npm install body-parser

2021년 이후 프로젝트에선 기본으로 내재되어 있다.

서버의 상단에 아래의 `app.use(express.urlencoded({extended: true}))`코드를 작성한다. `body-parser`를 실행하기 위한 초기 작업이다.

```jsx
cosnt bodyParser = require("body-parser");
app.use(bodyParser.urlencoded({extended: true}));
```

데이터를 전송할 `input`에는 `name` 속성과 값이 들어가야 한다. 들어갔다면 아래와 같이 사용할 수 있다.

```jsx
app.post("/add", function(req, res) {
	res.send(req.body.title);
}
```
