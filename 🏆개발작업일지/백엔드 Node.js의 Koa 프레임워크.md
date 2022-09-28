# 백엔드

<p>우리는  서버를 만들어 데이터를 여러 사람들과 공유합니다. 그런데 서버에 데이터를 무작정 담지는 않습니다. 데이터를 담을 때는 여러 규칙이 필요합니다

<h3>특정 데이터를 등록할 때
<li>사용자의 인증 정보가 필요할 수 있다,
<li>등록할 데이터를 어떻게 검증할지
<li>데이터의 종류가 다양하다면 어떻게 구분할지 등을 고려해야한다

<h3>특정 데이터를 조회할 떄 
<li>어떤 종류의 데이터를 몇 개씩 보여줄지, 또 어떻게 보여줄지 등에 관한 로직을 만드는 것을 서버 프로그래밍 또는 백엔드 프로그래밍 이라고합니다.

## Node. js  

<p>Node.js는 처음에 자바스크립트를 웹 브라우저만 사용했습니다.  속도가 그렇게 빠르지 않았습니다. 하지만 시간이 지나면서 구글이 크롬 웹 브라우저를 소개하면서 V8 이라는 자바스크립트 엔진을 공개했습니다. 자바스크립트 엔젠을 기반으로 웹 브라우저뿐만 아니라 서버에서도 자바스크립트를 사용 할 수 있는 런타임 개발했는데, 이것이 바로 Node.js 입니다.


![[Pasted image 20220924165156.png]]



## Koa

<p> Koa 는 Express 프레임워크 의 기존 개발팀이 개발한 프레임워크입니다. 기존 Express에서 고치고 싶었던 점들을 개선하면 내부 설계가 완전히 바뀌기 때문에 개발 팀이 아예 새로운 프레임워크를 개발했다고 한다.

<p> Express는 미들웨어, 라우팅, 템플릿, 파일 호스팅 등과 같은 다양한 기능이 자체적으로 내장되어 있는 반면, Koa는 미들웨어 기능만 갖추고 있다.  Koa는 우리가 필요한 기능들만 붙혀서 서버를 만들 수 있기 때문에 Express 보다 가볍다, 추가로 Koa는 async/await 문법을 정식으로 지원하기 때문에 비동기 작업을 더 편하게 관리 할 수 있다.

![[Pasted image 20220924165758.png]]


## Node.js 기초 실습

<li>작업 환경 준비
<li>Koa 서버 띄우기
<li>미들웨어 알아보기
<li>koa-router를 통한 백엔드 라우팅
<li>라우트 모둘화하기

## 2.1 작업 환경 준비

<p> Node.js 개발을 하기 위해서는 당연히 Node.js 런타임이 설치 되어져있어야 합니다.


### 2.2 프로젝트 생성

<p> 이 책에서 다루는 마지막 프로젝트인 블로그 서비스와 연동할 서버입니다. blog 디렉터리를 만들고, 그 내부에 blog-backend 디렉터리를 만들고 , 해당 디렉터리에서 yarn init -y 명령을 실행시켜 패키지 정보를 생성하세요 아래와 같이 실행하시면 됩니다.


![[Pasted image 20220924170535.png]]

<p>이 작업을 하고 나면 디렉터리에 package.json 파일이 생성됩니다. 다음 명령어를 실행하여서 해당 파일이 잘 만들어 졌는지 확인해보세요

	cat package.json

![[Pasted image 20220924170728.png]]

<p>이제 koa 웹 프레임워크를 설치해보겠습니다.

	yarn add koa


![[Pasted image 20220924170907.png]]


### 2.3 ESLint 와 Prettier 설정

<p> 서버 파일을 작성하기 전에 ESLint 와 Prettier를 프로젝트에 적용하겠습니다. 자바 스크립트 문법을 검사하고 깔끔한 코드를 작성하기 위해서 입니다.

<p> 두 기능을 Vs code 에서 사용하려면 Vs code 마켓플레이스에서 Prettier-Code formatter와 ESLint 확장 프로그램을 설치 해둔 상태여야합니다.

<h4> ESLint 설치입니다.
	<p>yarn add --dev eslint 
	<p>yarn run eslint --init


![[Pasted image 20220924171642.png]]


<p> 설치를 하시면 .eslintrc.json 파일 이 생성되었을 겁니다.

![[Pasted image 20220924171930.png]]


<p> 이제 Prettier를 설정하겠습니다. blog-backend 디렉터리에 다음 파일을 만드세요.

![[Pasted image 20220924172459.png]]

<p> 기능 적 설명은  상위 디렉터리에 CSS 디렉터리에 있으니 참고 하시면됩니다.
<p> 다음으로는 Prettier에서 관리하는 코드 스타일을 ESLint에서 관리하지 않도록 eslint-config-prettier를 설치하여 적용하세요

	yarn add eslint-config-prettier

<p> 설치를 하신다음에 src /index.js 파일어 만드세요.

![[Pasted image 20220924173017.png]]

<p>const로 값을 선언하고 사용하지 않으면, ESLint 기본 설정은 이를 에러로 간주합니다.
<p>사용하지 되지 않는 const 값은 문법적으로 문제없지만, 더 나은 코드를 작성하도록 장려하기 위해 ESLint는 이를 오류로 취급합니다. 이러한 규칙을 끌 수도있습니다. 오류 이름을 알아두면 .eslintrc.json에서 해당 오류를 경고로 바꾸거나 비활성화 할 수있습니다.

![[Pasted image 20220924174525.png]]


## Koa 기본 사용법

###   3.1 서버띄우기

	const Koa = require('koa');
	const app = new Koa();
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	}); //ctx은 매게 변수
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트

<p>위에 같이 /src/index.js 파일을 작성하시면 서버를 포트 4000번을 열고, 서버에 접속하면 Hello world 라는 텍스트로 반환시켜줍니다. 그 후서버 시켜줍니다  

<p>웹 브라우저로 http://localhost:4000/ 접속하세요

	node src

![[Pasted image 20220924185112.png]]

<p> 위와 같이 실행이 됩니다.

### 3.2 미들웨어

<p> Koa 애플리케이션은 미들웨어의 배열로 구성되어있습니다. 위의 app.use 함수를 사용한거 처럼 이 함수는 미들웨어 함수를 애플리케이션에 등록합니다.

<h4>미들웨어 함수 구조

	(ctx,  next) => {
	}

<p> koa의 미들웨어 함수는 두 개의 파라미터를 받습니다.  첫 번째 파라미터는 위에 사용하였던 ctx 값이고 , 두번째 파라미터는 next 입니다.

<h4>ctx 란 ? 
<p>Context의 줄임말로 웹 요청과 응답에 관한 정보를 지니고 있습니다.

<h4>next란 ?
	<li>현재 처리중인 미들웨어의 다음 미들웨어을 호출하는 함수입니다. 
	<li>미들웨어를 등록하고 next 함수를 호출하지 않으면, 그다음 미들웨어를 처리하지 않습니다.
	<li>미들웨어는 app.use를 사용하여 등록되는 순서대로 처리됩니다.

<p> 다음과 같이 현재 요청을 받은 주소와 우리가 정해 준 숫자를 기록하는 두 개의 미들웨어 src/index

	const Koa = require('koa');
	const app = new Koa();
	
	app.use((ctx,next)=> {
	console.log(ctx.url);
	console.log(1);
	next();
	});
	
	app.use((ctx, next) => {
	console.log(2);
	next();
	});
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	}); //ctx은 매게 변수
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트

<p> 다시 node src 명령어 실행하면 터미널에 아래와 같은 결과물이 나타납니다.

![[Pasted image 20220925021450.png]]

<p> 크롬 브라우저는 사용자가 웹 페이지에 들어가면 해당 사이트의 아이콘 파일인 /favicon.ico 파일을 서버에 요청하기 때문에 결과에 / 경로도 나타나고 /favicon.ico 경로도 나타납니다.

<p>이번에는 첫 번째 미들웨어에서 호출하던 next 함수를 주석으로 처리하겠습니다.

	const Koa = require('koa');
	const app = new Koa();
	
	app.use((ctx,next)=> {
	console.log(ctx.url);
	console.log(1);
	//next();
	});
	
	app.use((ctx, next) => {
	console.log(2);
	next();
	});
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	});
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트


![[Pasted image 20220925200559.png]]

<p> next를 호출하지 않으니 첫 번째 미들웨어까지만 실행하고 그 아래에 있는 미들웨어는 모두 무시 되었습니다.

<p> 이런 속성을 사용하여 조건부로 다음 미들웨어 처리를 무시하게 만들 수 있다. 다음 코드에서는 요청 경로에 authorized =1 이라는 쿼리 파라미터가 포함되어 있으면 이후 미들웨어를 처리해 주고, 그렇지 않으면 이후 미들웨어를 처리하지 않습니다.


	const Koa = require('koa');
	const app = new Koa();
	
	app.use((ctx,next)=> {
	console.log(ctx.url);
	console.log(1);
	if(ctx.query.authorized !== '1'){
		ctx.status = 401; //Unauthorized
		return;
	}
	
	next();
	});
	
	app.use((ctx, next) => {
	console.log(2);
	next();
	});
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	});
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트


<p>쿼터 파라미터는 문자열이기 때문에 비교할 때는 꼭 문자열 형태로 비교해야합니다. 이제는 서버를 재 시작한 뒤 다음 링크에 들어가서 어떤 결과가 나타 나는지 확인해 보세요.

<li>http://localhost:4000/
<br>
<li>http://localhost:4000/?authorized=1

### 3.2.1 next 함수는 Promise를 반환

<p>next 함수를 호출하면 Promise를 반환합니다. 이는 Koa가 Express 와 차별화되는 부분입니다.
<p>next 함수가 반환하는 Promise는 다음에 처리해야 할 미들웨어가 끝나야 완료됩니다. 다음과 같이 next 함수 호출 이후에 then을 사용하여 Promise가 끝난 다음 콘솔에 END를 기록하도록 수정해보기

	const Koa = require('koa');
	const app = new Koa();
	
	app.use((ctx,next)=> {
	console.log(ctx.url);
	console.log(1);
	if(ctx.query.authorized !== '1'){
		ctx.status = 401; //Unauthorized
		return;
	}
	next().then(() =>;{
		console.log('END');
	});
	});
	
	app.use((ctx, next) => {
	console.log(2);
	next();
	});
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	});
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트

![[Pasted image 20220925210111.png]]


### 3.2.2 async/await 사용하기

<p>Koa는 async/await를 정식으로 지원하기 때문에 해당 문법을 아주 편하게 사용할 수 있습니다.

	(메모) 서버 사이드 렌더링을 할 때 사용했던 Express도 async/await 문법을 사용할 수잇지만, 오류를 처리하는 부분이 제대로 작동하지 않을 수 있습니다. 백엔드 개발을 하면서 예상치 못한 에러를 제대로 잡아내려면 express-async-errors라는 라이브러리를 따로 사용해야합니다.



	const Koa = require('koa');
	const app = new Koa();
	
	app.use(async(ctx,next)=> {
	console.log(ctx.url);
	console.log(1);
	if(ctx.query.authorized !== '1'){
		ctx.status = 401; //Unauthorized
		return;
	}
	await next();
		console.log('END');
	});
	
	app.use((ctx, next) => {
	console.log(2);
	next();
	});
	
	app.use(ctx => {
	ctx.body = 'hello world!';
	});
	//hello world라는 텍스트를 반환하도록 설정
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트
	
![[Pasted image 20220925210726.png]]

## 4. nodemon 사용하기

<p>서버 코드를 변경할 때마다 서버를 재시작하는 것이 번거롭기 때문에 nodemon이라는 도구를 사용하면 코드를 변경할 때마다 서버를 자동으로 재시작해줍니다.

	yarn add --dev nodemon

<p> 그다음 package.json scripts를 다음과 같이 입력하세요.

![[Pasted image 20220925211600.png]]

<p>start 스크립트에는 서버를 시작하는 명령어를 넣고, start:dev 스크립트에는 nodemon을 통해 서버를 실행해 주는 명령어를 넣습니다. 여기서 nodemon은 src 디렉터리를 주시하고 있다가 해당 디렉터리 내부의 어떤 파일이 변경되면, 이를 감지하고 src/index.js 파일을 재시작 해줍니다.

<li>	# yarn start # 재시작이 필요 없을 때
<li> # yarn start : dev # 재시작이 필요할 때

![[Pasted image 20220925212256.png]]

<p>파일을 저장할 때 위와 같이 터미널에 출력된다


## 5. koa-router 사용하기



<p> 앞에서 리액트를 배울 때 웹 브라우저의 라우ㅇ을 돕는 리액트 라우터 라브러리를 사용해 보았습니다. Koa를 사용 할 때도 다른 주소로 요청이 들어올 경우 다른 작업을 처리할 수 있도록 라우터를 사용해야 합니다. Koa 자체에 이 기능이 내장되어 잇지 않으므로, Koa-router 모듈을 설치해야 합니다.

	yarn add koa-router

### 5.1 기본 사용법

<p>index.js에서 라우터를 불러와 적용하는 방법입니다.

	const Koa = require('koa');
	const Router = new Router();

	//라우터 설정
	router.get('/', ctx =>{
	ctx.body = '홈';
	})
	router.get('/about', ctx=>{
	ctx.body= "소개";
	});

	//app 인스턴스에 라우터 적용
	app.use(router.routes()).use(router.allowMethods());

	app.listen(4000, () => { 
		console.log('Listening to port 4000');
	})


<p>koa-router를 불러온 뒤 이를 사용하여 Router 인스턴스를 만들었습니다. 그리고 / 경로로 들어오면 '홈'을 띄우고, /about 경로로들어오면 '소개' 를 텍스로 나타나도록 설정하였습니다.

<p>이처럼 라우트를 설정할 떄, router.get의 첫번째 파라미터에는 라우트의 경로 , 두번 째 파라미터에는 해당 라우트에 적용할 미드웨어 함수!!를 넣습니다. 여기서 get키워드는 해당 라우트에서 사용할 HTTP 메서드를 의미합니다. get 대신에 post,put,delete 등을 넣을 수있습니다.

### 5.2 라우트 파라미터와  쿼리

<p> 라우터의 파라미터의 설정 할 때는 /about/:name 형식으로 콜론( ; )을 사용하여 라우트 경로를 설정합니다. 또 파라미터가 있을 수도 있고 없을 수도 있다면 /about/name? 같은 형식으로 파라미터 이름 뒤에 물음표를 사용합니다. 이렇게 설정한  파라미터는 함수의 ctx.params 객체에서 조회합니다.

<p>URL 쿼리의 경우, 예를 들어 /posts/?id=10 같은 형식으로 요청했다면 해당 값을 ctx.query에서 조회 할 수 있습니다. 쿼리 문자열을 자동으로 객체 형태로 파싱해 주므로 별도로 파싱 함수를 돌릴 필요가 없습니다.(문자열 형태의 쿼리 문자열을 조회해야 할 때는 ctx.querystring을 사용합니다. )

	const Koa = require('koa');
	const Router = require('koa-router');

	const app = new Koa();
	const router = new Router();
	
	//라우터 설정
	router.get('/', ctx =>{
	ctx.body = '홈';
	})
	router.get('/about/:name?',ctx=>{
	const {name} = ctx.params;
	//name의 존재 유무에 따라 다른결과 소개
	ctx.body = name ? `${name}의 소개` : '소개';
	});
	router.get('/posts',ctx => {
	const { id } = ctx.query;
	// id의 존재 유무에 따라 다른 결과 출력
	ctx.body = id ? `포스트# ${id}` : '포스트 아이디가 없습니다.';
	})
	
	//app 인스턴스에 라우터 적용
	app.use(router.routes()).use(router.allowedMethods());
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트

<p> 코드를 작성하고 다음 아래의 링크로 들어가시요

<li>http://localhost:4000/about/react
<li>http://localhost:4000/posts
<li>http://localhost:4000/posts?=id

![[Pasted image 20220927153653.png]]

![[Pasted image 20220927153709.png]]

![[Pasted image 20220927153718.png]]

<p>파라미터와 쿼리는 둘 다 주소를 통해 특정 값을 받아 올 때 사용하지만, 용도가 서로 조금씩 다릅니다. 정해진 규칙은 따로 없지만, 일반적으로 파라미터는 처리할 작업의 카테고리를 받아 오거나 , 고유 ID 혹은 이름으로 특정 데이터를 조회할 때 사용합니다. 반면, 쿼리는 옵션에 관련된 정보를 받아 옵니다. 예를 들어 여러 항목을 리스팅하는 API라면, 어떤 조건을 만족하는 항목을 보여 줄지 또는 어떤 기준으로 정렬할지를 정해야 할 때 쿼리를 사용합니다.


### 5.3 REST API
<p>웹 애플리케이션을 만들려면 데이터베이스에 정보를 입력하고 읽어 와야 합니다. 그런데 웹 브라우저에서 데이터베이스에 직접 접속하여 데이터를 변경한다면 보안상 문제가 되니깐 그래서 REST API를 만들어서 사용합니다.

![[Pasted image 20220927155033.png]]

<p>클라이언트가 서버에 자신이 데이터를 조회 , 생성, 삭제, 업데이트 하겠다고 요청하면, 서버는 필요한 로직에 따라 데이터베이스에 접근하여 직업을 처리합니다.

<p>REST API는 요청 종류에 따라 다른 HTTP 메서드를 사용합니다. HTTP 메서드는 여러 종류가 있으며 , 주로 사용하는 메서드는 아래와 같습니다.
<br>

<h4>HTTP 메서드의 종류
<li>GET : 데이터를 조회할 때 사용합니다.
<li>POST : 데이터를 등록할 때 사용합니다. 인증 작업을 거칠 때 사용하기도 합니다.
<li>DELETE : 데이터를 지울 때 사용합니다.
<li>PUT : 데이터를 새 정보로 통재로 교체할 때 사용합니다.
<li>PATCH  : 데이터의 특정 필드를 수정할 때 사용합니다.

<p>메서드의 종류에 따라 get,post, delete, put , patch를 사용하여 라우터에서 각 메서드의 요청을 처리합니다. 

<p>REST API를 설계할 때는 API 주소와 메서드에 따라 어떤 역할을 하는지 쉽게 파악할 수 있도록 적상 해야합니다. 


<p>블로그 포스트용 REST API
<li> POST /posts  : 포스트 작성
<li>GET /posts  : 포스트 목록 조회
<li>GET /posts/:id  : 특정 포스트 조회
<li>DELETE /posts/:id   : 특정 포스트 삭제
<li>PATCH /posts/:id   : 특정 포스트 업데이트(구현 방식에 따라 PUT으로도 사용 가능)
<li>POST /posts/:id/comments    : 특정 포스트에 덧글 등록
<li>GET / posts/:id/comments  : 특정 포스트의 덧글 목록 조회
<li>DELETE /posts/:id/comment/:commentID :  특정 포스트의 특정 덧글 삭제

### 5.4 라우터 모듈화

<p> 프로젝트를 진행하다 보면 여러 종류의 라우트를 만들게 됩니다. 각 라우트를 index.js 파일 하나에 모두 작성하면, 코드가 너무 길어질 뿐 아니라 유지보수하기도 힘들어 집니다. 여기서는 라우터를 여러 파일로 분리시켜서 작성하고, 이를 불러와 적용하는 방법을 해보겠습니다.

<p>src 디렉터리에 api디렉터리를 생성하고, 그안에 index.js 파일을 만들었습니다.


<p>src/api/index.js

		const Router = require('koa-router');
		const api = new Router;
		
		api.get('/test', ctx=>{
		ctx.body = 'test 성공';
		});
		
		//라우터로 보냅니다.
		module.exports = api;

<p>src/index.js

	const Koa = require('koa');
	const Router = require('koa-router');
	const api = require('./api');
	
	const app = new Koa();
	const router = new Router();
	
	//라우터 설정
	router.use('/api', api.routes());
	
	//app 인스턴스에 라우터 적용
	app.use(router.routes()).use(router.allowedMethods());
	
	app.listen(4000, () => {
	console.log('Listening to port 4000');
	}) //서버 포트는 4000번 포트

<p>우리가 만든 api 라우터를 서버의 매인 라우터의 /api 경로로 설정했습니다. 따라서 /api/test 경로로 요청하면 조금 전에 준비했던 'test 성공 ' 문자열이 나타날 것입니다.

![[Pasted image 20220927171838.png]]

### 5.5 posts 라우트 생성

<p> 이번에는 api 라우트 내부에 posts 라우트를 만들어 보겠습니다. api 디렉터리에 posts 디렉터리를 만들고 , 그 내부에 index.js 파일만들겠습니다.

<p>src/api/posts/index.js
	const Router = require('koa-router');
	const posts = new Router();  

	const printInfo = ctx => {
		ctx.body = {
			method : ctx.method,
			path : ctx.path,
			params : ctx.params,
		};
	};
	
		posts.get('/', printInfo);
	posts.post('/',printInfo);
	posts.get('/:id', printInfo);
	posts.delete('/:id', printInfo);
	posts.put('/:id', printInfo);
	posts.path('/:id', printInfo);
	module.exports = posts;

<p>posts 라우트에 여러 종류의 라우트를 설정한 후 모든 printInfo 함수를 호출하도록 설정하였습니다. 문자열이 아닌 JSON 객체를 반환하도록 설정하고, 이 객체는 현재 요청의 메서드 , 경로, 파라미터를 담았습니다.

<p> 코드를 완성한 후, api 라우트에 posts 라우트를 연결합니다. 연결하는 방법은 서버의 메인 파일에 api 라우트를 적용하는 방법과 비슷합니다.

<p>src/api/index.js

	const Router = require('koa-router');
	const posts = require('./posts')
	
	const api = new Router;
	api.use('/posts', posts.routes() )
	
	//라우터로 보냅니다.
	module.exports = api;
	
<p>기존 test 라우트는 지우고, posts 라우트를 불러와서 설정해 주었습니다. 우선 GET/ api/posts 라우터부터 테스트 해보겠습니다.

![[Pasted image 20220927175401.png]]


### 5.5.1 Postman 의 설치 및 사용

<p> Postman은 macOS, Windows,  리눅스에서 모두 사용할 수 있는 프로그램이다. 이프로그램은 https://www.postman.com/ 에서 인스톨러로 설치할수 있습니다. GET 셀렉트 박스를 클릭하여 메서드 선택한후 Send 버튼을 누르면 요청 할 수있다.


![[Pasted image 20220928184904.png]]


### 5.5.2 컨트롤러 파일 작성

<p> 라우트를 작성하는 과정에서 특징 경로에 미들웨어를 등록 할 때는 다음과 같이 두 번째 인자에 함수를 선언해서 바로 넣을 줄 수 있습니다.

	router.get('/', ctx= { 
	});

<p> 각 라우트 처리 함수가 코드가 길면 라우터 설정을 한눈에 보기 힘들다. 그렇게 때문에 이 라우트 처리 함수들을 다른 파일로 따로 분리해서 관리할 수 있습니다. 이 라우트 처리 함수만 모아 놓은 파일을 컨트롤러라고 합니다.  

<p> 지금은 아직 데이터베이스를 연결하지 않았으므로 자바스크립트의 배열 기능만 사용하여 임시로 기능을 구현해보았습니다.

<p>API 기능을 본젹적으로 구현하기 전에 먼저 koa-bodyparser 미들웨어를 적용해야 합니다. 이 미들웨어는 POST/PUT/PATCH 같은 메서드의 Requset Body에 JSON 형식으로 데이터를 넣어 주면, 이를 파싱하여 서버에서 사용할 수 있다.

		yarn add koa-bodyparser

<p>이어서 미들웨어를 불러와 적용하세요 . src/index.js

	const Router = require('koa-router');
	const postsCtrl = require('./posts.ctrl')
	
	const posts = new Router();
	
	const printInfo = ctx => {
	ctx.body = {
	method : ctx.method,
	path : ctx.path,
	params : ctx.params,
		};
	};
	posts.get('/', postsCtrl.list);
	posts.post('/',postsCtrl.write);
	posts.get('/:id', postsCtrl.read);
	posts.delete('/:id',postsCtrl.remove);
	posts.put('/:id',postsCtrl.replace);
	posts.patch('/:id',postsCtrl.update);
	module.exports = posts;

<p>posts 컨트롤러 입니다 . ->posts/posts.ctrl.js

	let postId = 1; //id의 초깃값입니다
	//posts 배열 초기 데이터
	const posts =[
	{
	id : 1,
	title : '제목',
	body : '내용',
	},
	];
	/*
	
	포스트 작성
	
	POST /api/posts
	
	{title, body}
	
	*/
	
	  
	
	exports.write = ctx => {
	
	//REST API의 Requst Body는 ctx.request.body 에서 조회 할수 있습니다.
	
	const{title, body} = ctx.request.body;
	
	postId += 1; //기존 postId 값에 1을 더합니다
	
	const post = {id : postId , title, body};
	
	posts.push(post);
	
	};
	
	  
	
	/*
	
	포스트 목록 조회
	
	GET /api /posts
	*/
	exports.list = ctx => {
	ctx.body = posts;
	};
	/*
	특정 포스트 조회
	GET /api /posts/:id
	*/
	
	exports.read = ctx => {
	
	const {id} = ctx.params;
	
	//주어진 id 값으로 포스트를 찾습ㄴ디ㅏ.
	
	//파라미터로 받아 온 값은 문자열 형식이므로 숫자로 변환하거나
	
	//비교할 p.id 값을 문자열로 변경해야 합니다.
	
	const post = posts.find(p=>p.id.toString() === id );
	
	if(!post){
	
	ctx.status = 404;
	
	ctx.body = {
	
	message : '포스트가 존재 하지 않습니다',
	
	};
	
	return;
	
	}
	
	ctx.body = post;
	
	};
	
	  
	
	/*
	
	포스트 목록 wprj
	
	DELETE /api /posts/:id
	
	*/
	
	  
	
	exports.remove = ctx=>{
	
	const {id} = ctx.params;
	
	//해당 id 를 가진 post가 몇 번째인지 확인합니다.
	
	const index = posts.findIndex(p =>p.id.toString() ==== id);
	
	//포스트가 없으면 오류를 반환합니다.
	
	if(index === -1){
	
	ctx.status =404;
	
	ctx.body = {
	
	message = '포스트가 존재하지 않습니다.'
	
	};
	
	return;
	
	}
	
	//index번째 아이템을 제거합니다.
	
	posts.splice(index,1);
	
	ctx.status = 204; //No Content
	
	};
	
	/*
	
	포스트 수정(교체)
	
	GET /api/posts/:id
	
	{title, body}
	
	*/
	
	  
	
	exports.replace = ctx => {
	
	//PUT 메서드는 전체 포스트 정보를 입력하여 데이터를 통째로 교체할 때 사용합니다.
	
	const {id} = ctx.params;
	
	//해당 id를 가진 post가 몇 번재인지 확인합니다.
	
	const index = posts.findIndex(p => p.id.toString() === id);
	
	//포스트가 없으면 오류를 반환합니다.
	
	if(index === -1){
	
	ctx.status = 404;
	
	ctx.body = {
	
	message : '포스트가 존재하지 않습니다.'
	
	};
	
	return;
	
	}
	
	//전체 객체를 덮어 씌웁니다.
	
	//따라서 id를 제외한 기존 정보를 날리고, 객체를 새로 만듭니다.
	
	post[index]= {
	
	id,
	
	...ctx.request.body,
	
	};
	
	ctx.body=posts[index];
	
	};
	
	  
	
	/*
	
	포스트 수정(특정 필드 변경)
	
	PATCH /api /posts/:id
	
	{title, body}
	
	*/
	
	exports.update = ctx => {
	
	//PATCH 메서드는 주어진 필드만 교체합니다.
	
	const { id } = ctx.params;
	
	//해당 id를 가진 post가 몇 번째 인지 확인합니다
	
	const index = posts.findIndex(p=>p.id.toString() === id);
	
	//포스트가 없으면 오류를 반환합니다.
	
	if(index === -1){
	
	ctx.status =404;
	
	ctx.body = {
	message : '포스트가 존재하지 않습니다.'
	};
	return;
	}
	//기존 값에 정보를 덮어 씌웁니다.
	posts[index] = {
	...posts[index],
	...ctx.request.body,
	};
	ctx.body = posts[index];
	}
	


<p>컨트롤러를 만들면서 exports. 이름 = ... 형식으로 함수를 내보내 주었습니다. 이렇게 내보낸 코드는 다음형식으로 볼수있습니다.

	const 모듈이름 = require('파일이름');
	모듈이름.이름();

<p> require('./posts.ctrl')을 입력하면 방금 만든 posts.ctrl.js 파일을 불러온다면 다음 객체를 불러오게 됩니다.
	
	{
	write : Funtion,
	list : Funtion,
	read :Funtion,
	remove : Funtion,
	replace :Funtion,
	update : Funtion,	
	}

<p>우리가 만든 컨트롤러 함수들을 한번 각 라우트에 연결시켜보겠습니다.
	const Router = require('koa-router');
	const postsCtrl = require('./posts.ctrl');
	
	const posts = new Router();
	
	const printInfo = ctx => {
	ctx.body = {
	method : ctx.method,
	path : ctx.path,
	params : ctx.params,
		};
	};
	posts.get('/', postsCtrl.list);
	posts.post('/',postsCtrl.write);
	posts.get('/:id', postsCtrl.read);
	posts.delete('/:id',postsCtrl.remove);
	posts.put('/:id',postsCtrl.replace);
	posts.patch('/:id',postsCtrl.update);
	module.exports = posts;

<p>이제 posts 라웥가 완성되었습니다.

<p>list , read, remove를 제외한 API들을 요청할 때 Request body가 필요한데,POostman에서 이 값을 어떻게 넣는지 알아보십니다.

Postman에서 POST를 선택하면 다음과 같이 body부분이 활성화됩니다. BODY탭을 선택하고 raw 옵션을 클릭한 후 주황색으로 나타내는 데이터 타입을 JSON으로 설정하세요. 그리고 하단 텍스트 박스에 다음 JSON을 입력하세요

	{
		"title" : "테스팅",
		"body" : "테스팅"
	}
<p>실행하면 아래 사진과같이 나옵니다

	
![[Pasted image 20220928204601.png]]

<p> 아래는 요청 PATCH

![[Pasted image 20220928204820.png]]


<p> PUT

![[Pasted image 20220928205021.png]]

<p>PUT 메서드를 사용하니 기존 body가 사라져 버렸습니다. 따라서 포스트 수정 API를 PUT으로 구현해야 할때는 모든 필드가 다 있는지 검증하는 작업이 필요합니다.


## 정리

<p>이장에서 Koa를 사용하여 백엔드 서버를 만드는 기본 개념을 대해 알아보았습니다. 먼저 REST API를 살펴 본후 어떻게 작동하는지를 자바스크립트 배열을 사용하여 구현하면서 알아보았습니다. 자바스크립트 배열을 사용하여 구현하며 서버를 재시작할 때 당연히 데이터가 소멸됩니다. 물론 이 데이터를 로컬 파일에 저장하는 방법도 있지만 , 그대신 MySQL, MongoDB 등의 데이터베이스에 정보를 저장하여 관리합니다.

