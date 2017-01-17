# Node & Express
## 익스프레스로 시간 절약 
### 초기 단계 
* 프로젝트에 사용할 새 디텍터리 생성.
* npm init (package.json 파일을 생성)
* npm install --save express (익스프레스 설치) - --save 플래그를 지정하면 package.json 파일을 업데이트 된다.
* ooo.js 파일 생성. 이 파일이 프로젝트의 입점이다.
```javascript
var express = require('express');
var app = express();

app.set('post', process.env.PORT || 3000);

// home
app.get('/'. function(req, res) {
  res.type('text/plain');
  res.send('home');
});

// about
app.get('/about'. function(req, res) {
  res.type('text/plain');
  res.send('about');
});

// 커스텀 404 페이지 
app.use(function(req, res) {
  res.type('text/plain');
  res.status(404);
  res.send('404 - Not Found');
});

// 커스텀 500 페이지 
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.type('text/plain');
  res.status(500);
  res.send('500 - Server Error');
});

app.listen(app.get('post'), function() {
  console.log('Express started on http://localhost:' + app.get('post') + '; press Ctrl-C to terminate.');
});
```
**익스프레스에서는 라우트와 미들웨어를 추가하는 순서가 중요하다. 404 핸들러를 라우트 앞에 두었다면 홈페이지와 어바웃 페이지는 동작하지 않고 404 에러가 일어난다.**
기초적인 익스프레스 서버 완료.
```node
node OOO.js //서버 시작
```
### 뷰와 레이아웃 
핸들바 설치
```node
npm install --save express-handlebars
```
생성한 OOO.js 에 다음 행을 추가한다.
```javascript
var app = express();

// 핸들바 뷰 엔진 설정 
var handlebars = require('express-handlebars').create({ defaultLayout: 'main' });
app.engine('handlebars', handlebars.engine);
app.set('view engine', ' handlebars');
```
* views/layouts/ 디렉토리를 만든다.
* main.handelbars 파일 생성.
```html
<!doctype html>
<html>
<head>
<title>title</title>
</head>
<body>
  {{{body}}}
</body>
</html>
```
OOO.js에서 핸들바 인스턴스를 만들면서 defaultLayout:'main'으로 기본 레이아웃을 지정. 따로 명시하지 않으면 모든 뷰에서 이 레이아웃을 쓰겠다는 의미.
home, about, 404, 500 페이지를 views 폴더에서 만든다.
뷰를 만들었으니 기존 라우터 수정.
```javascript
// home
app.get('/'. function(req, res) {
  res.type('text/plain');
  res.render('home');
});

// about
app.get('/about'. function(req, res) {
  res.type('text/plain');
  res.render('about');
});

// 커스텀 404 페이지 
app.use(function(req, res) {
  res.type('text/plain');
  res.status(404);
  res.render('404');
});
...
```

### 정적 파일과 뷰 
익스프레스는 미들웨어를 통해 정적 파일오가 뷰를 처리한다.
* 프로젝트 디렉터리 안에 public 서브 디렉터리 생성.
* static 미들웨어 추가 
```javascript
app.use(express.static(__dirname + '/public'));
```

## 품질보증 
### 페이지 테스트
테스트 프레임워크 모카 
```node
npm install --save-dev mocha
```
--save가 아니라 --save-dev 옵션을 사용한 이유는 실무 환경이 아니라 개발할 때만 사용한다고 알리는 옵션.

## 요청과 응답 객체 
### URL의 각 부분 
*[프로토콜]*
프로토콜은 요청이 어떻게 전송될지 결정한다.

*[호스트]*
호스트는 서버를 가리킨다.

*[포토]*
*[경로]*
*[쿼리스트링]*
*[해시]*

### HTTP 요청 규칙 
