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

### 3.1 서버띄우기

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

###3.2.1 next 함수는 Promise를 반환

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

