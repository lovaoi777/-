# MongoDB
<p>우리 만들 Node.js 서버와 MongoDB를 연동할 수 있도록 MongoDB 기초 지식을 알아봅니다. 그리고 monogoose를 이용히여 서버에서 직접 데이터를 추가, 조회, 삭제 , 수정 하는 방법도 알아보겠습니다.

<h3>이번 실습의 흐름입니다.

<li>MongoDB 기본 지식 알아보기
<li>작업 환경 설정하기
<li>mongoose로 데이터베이스 연결하기
<li>esm으로 ES 모듈 import/export 문법 사용하기
<li>스키마와 모델 이해하기
<li>REST API 구현하기
<li>페이지네이션 구현하기

## MongoDB 기본 지식 알아보기

<p>서버를 개발할 때 데이터베이스를 사용하면 웹 서비스에서 사용되는 데이터를 저장하고, 효율적으로 조회하거나 수정할 수 있습니다. 기존에는 MySQL, OracleDB, PostgreSQL 같은 RDBMS(관계형 데이터베이스를) 자주 사용했습니다.

<p>❗️그런데 관계형 데이터베이스는 몇 가지 한계가 있습니다.

<p>1. 데이터 스키마가 고정적이라는 것입니다. 

	여기서 스키마란 데이터베이스에 어떤 형식의 데이터를 넣을지에 대한 정보를 가르킵니다. 예를 들어 회원 정보 스키마라면 계정명, 이메일. 이름 등이 입니다. 새로 등록하는 데이터 형식이 기존에 있던 데이터들과 다르다면? 기존 데이터를 모두 수정해야 새 데이터를 등록할 수 있습니다. 그래서 데이터양이 많을 때는 데이터베이스의 스키마를 변경하는 작업이 매우 번거로워질 수 있습니다.

<p>2. 확장성입니다.

	RDBMS(관계형 데이터베이스)는 저장하고 처리해야 할 데이터양이 늘어나면 여러 컴퓨터에 분산시키는 것이 아니라, 해당 데이터베이스 서버의 성능을 업그레이드하는 방식으로 확정해 주어야 했습니다.

<hr>

<p>MongoDB는 이런 한계를 극복한 문서 지향형 NoSQL 데이터베이스 입니다. 이 데이터베이스에 등록하는 데이터들은 유도적인 스키마를 지닐 수 있습니다. 종류가 같은 데이터라고 하더라도, 새로운 등록해야 할 데이터 형식이 바뀐다고 하더라도 기존 데이터까지 수정할 필요가 없습니다. 서버의 데이터양이 늘어나도 한 컴퓨터에서만 처리하는 것이 아니라 여러 컴퓨터로 분산하여 처리할 수 있도록 확장하기 쉽게 설계되어 있습니다.

<p>❗️MongoDB가 무조건 기존의 RDBMS보다 좋은 것은 압니다. 상황별로 적합한 데이터베이스가 다를수도 있습니다.  예를 들어 데이터의 구조가 자주 바뀐다면 MongoDB가 유리하고, 까다로운 조건으로 데이터를 필터링 해야 하거나, ACID 특성을 지켜야 한다면 RDBMS가 더  유리 할 수 있습니다.

	ACID 특성은 원자성(Atomicity), 일관성(Consistency), 고립성(lsolation), 지속성(Durability)의 앞글자를 따서 만든 용어로, 데티베이스 트랜잭션이 안전하게 처리되는 것을 보장하기 위한 성질입니다.


### 1.1 문서란?

<p> 여기서 말하는 '문서(document)'는 RDBMS의 레코드(record)와 개념이 비슷합니다. 문서의 데이터 구조는 한 개 이상의 키-값 쌍으로 되어 있습니다.

<p>MongoDB에서 사용하는 문서 예시 

	{
		"_id": ObjectId("5099803df3f4948bd2f98391"),
		"username" : "velopert",
		"name" : {firest  : "M.J", last : "Kim" }
	}

<p> 문서는 BSON(바이너리 형태의 JSON) 형태로 저장된다. 그렇기 때문에 나중에 JSON 형태의 객체를 데이터베이스에 저장할 때, 큰 공수를 들이지 않고도 데이터를 데이터베이스에 등록할 수 있어 매우 편하다.

<p>새로운 문서를 만들면 _id 라는 고윳값을 자동으로 생성하는데, 이 값은 시간, 머신 아이디, 프로세스 아이디, 순차 번호로 되어 있어 값의 고유함을 보장합니다.

<p>여러 문서가 들어 있는 곳을 컬렉션이라고한다. 기존 RDBMS에서는 테이블 개념을 사용하므로 각 테이블마다 같은 스키마를 가지고 있어야 한다. 새로 등록해야 할 데이터가 다른 스키마를 가지고 있다면, 기존 데이터들의 스키마도 모두 바꾸어 주어야한다.

<p>반면 MongoDB는 다른 스키마를 가지고 있는 문서들이 한 컬렉션에서 공존할 수있다. 아래의 예시를 살펴 보세요.

	{
	"_id" :  ObjectId("59498a081ad6e0ea526f3f5"),
	"username" : "velopert"
	},
	{
	"_id" : ObjectId("59494fca81ad6e0ea526f3f6"),
	"username":"velopert2",
	"phone" : "010-1234-1234",
	}

<p>처음에는 데이터전화번호가 필요가 없었는데, 나중에 필요해졌다고 가정해봅시다. RDBMS에서는 한 테이블의 모든 데이터가 같은 스키마를 가져야 하기 때문에, 기존 데이터 전체를 일일이 수정해야합니다. 하지만, MongoDB에서는 컬렉션 안의 데이터가 같은 스키마를 가질 필요가 없으므르 그냥 넣어 주면 됩니다.

### 1.2 MongoDB 구조

<p> MongoDB 구조는 다음과 같습니다. 서버 하나에 데이터베이스를 여러 개 가지고 있을 수 있습니다. 각 데이터베이스에서는 열 개의 컬렉션이 있으며, 컬렉션 내부에는 문서들이 들어 있습니다.

![[Pasted image 20221004172927.png]]

### 1.3 스키마 디자인

<p> MongoDB에서 스키마를 디자인 하는 방식은 기존 RDBMS에서 스키마를 디자인하는 방식과 완전히 다르다. RDBMS에서 블로그용 데이터 스키마를 설계한다면 각 포스트, 댓글마다 테이블을 만들어 필요에 따라 JOIN해서 사용하는것이 일반적입니다.

<p>RDBMS에서 데이터베이스를 설계한다면 그 구조는 다음과 유사합니다.

![[Pasted image 20221004173125.png]]

<p>하지만 NoSQL 에서는 그냥 모든 것을 문서 하나에 넣습니다. 문서 예시의 형식을 한번 살펴볼까요 ?

	{
		_id : ObjectId,
		title : String,
		body : String, 
		username : String,
		createDate : Date,
		comments : [
		{
			_id : ObjectId,
			text : String,
			createdDate : Date,
		},
		],
	};

<p>이런 상황에서 보통 MongoDB는 댓글을 포스트 문서 내부에 넣습니다. 문서 내부에 또 다른 문서가 위치 할수있는데, 이를 서브다큐먼트(subdocument) 라고합니다. 서브다큐먼트 또한 이란문서를 다루는 것처럼 쿼리할 수있다.

<p>문서 하나에 최대 16MB만큼 데이터를 넣을 수 있는데요. 100자 댓글 데이터라면 대략 0.24KB를 차지합니다. 16MB는 16.384KB이니 문서 하나에 댓글 데이터를 약 68,000개 넣을 수 있다.

<p> 서브다큐먼트에서 이 용량을 초과할  가능성이 있다면 컬렉션을 분리시키는 것이 좋습니다.


## MongoDB 서버 준비

### 2.1 설치

<p> macOS 에서는 Homebrew를 이용하여 간편하게 설치할 수 있습니다.

	$ brew tap mongodb/brew
	$ brew install mongodb-community@4.2
	$ brew services start mongodb-community@4.2

### 2.2 MongoDB 작동 확인

<p> MongoDB가 성공적으로 설치되었고 제대로 가동 중인지 확인하려면, 터미널에서 mongo를 입력해보세요

	mongo

<p>❗️실행 화면 

![[Pasted image 20221004180534.png]]


### 2.3 Mongoose의 설치 및 적용

<p> mongoose는 Node.js 환경에서는 사용하는 MongoDB 기반 ODM(Object Data Modelling) 라이브러리입니다. 이 라이브러리는 데이터베이스 문서들을 자바스크립트 객체처럼 사용할 수 있게 해줍니다.

<p>프로젝트 디렉터링서 다음 명령어를 입력하여 mongoose와 dotenv를 설치하세요.

<p>👍dotenv는 환경변수들을 파일에 넣고 사용할 수 있게 하는 개발 도구입니다. mongoose를 사용하여 MongoDB에 접속할 때, 서버에 주소나 계정 및 비밀번호가 필요할 경우도 있습니다. 이렇게 민감하거나 환경별로 달라질 수 있는 값은 코드안에 직접 작성하지 않고, 환경변수로 설정하는 것이 좋습니다.


#### 2.3.1 .env 환경변수 파일 생성

<p> 환경변수에는 서버에서 사용할 포트와 MongoDB 주소를 넣어 주겠습니다. 프로젝트의 루트 경로에 .env 파일을 만들고 다음 내용을 입력하세요

<p>.env

	PORT=4000
	MONGO_URL=mongodb://localhost:27017/blog

<p>여기서 blog는 우리가 사용할 데이터베이스 이름입니다. 지정한 데이터베이스가 서버에 없다면 자동으로 만들어 주므로 사전에 직접 생성할 필요는 없습니다.

<p> 다음으로 src/index.js 파일의 맨 위 다음과 같이 dotenv를 불러와서 config() 함수를 호출해주세요. Node.js에서 환경변수는 process.env 값을 통해 조회할 수 있습니다.

	require('dotenv').config();
	const Koa = require('koa');
	const Router = require('koa-router');
	const bodyParser = require('koa-bodyparser');
	
	
	//비구조화 할당을 통해 process.env 내부 값에 대한 래퍼런스 만들기
	// eslint-disable-next-line no-undef
	const { PORT } = process.env;
	const api = require('./api');
	const app = new Koa();
	const router = new Router();
	//라우터 설정
	router.use('/api', api.routes());
	
	//라우터 적용 전에 bodyParser적용
	app.use(bodyParser());
	
	//app 인스턴스에 라우터 적용
	app.use(router.routes()).use(router.allowedMethods());
	
	//PORT가 지정되지 있지 않다면 4000을 사용
	const port = PORT || 4000;
	app.listen(port, ()=>{
	console.log('Listening to port %d',port);
	});

<p>.env 파일에서 PORT를 4001로 변경한 뒤 서버를 한번 재시작해 보세요. .env 파일을 변경할 때는 nodemon에서 자동으로 재시작하지 않으므로 직접 재시작해야 합니다.

	Listening to port 4001


#### 2.3.2 Mongoose 로 서버와 데이터베이스 연결

<p> 이제 mongoose를 이용하여 서버와 데이터베이스를 연결하겠습니다. 연결할 때는 mongoose의 connect 함수를 사용합니다.
	
	require('dotenv').config();
	const Koa = require('koa');
	const Router = require('koa-router');
	const bodyParser = require('koa-bodyparser');
	const mongoose = require('mongoose');
	//비구조화 할당을 통해 process.env 내부 값에 대한 래퍼런스 만들기
	// eslint-disable-next-line no-undef
	const { PORT,MONGO_URI } = process.env;
	
	mongoose
	.connect(MONGO_URI)
	.then(()=>{
	console.log('Connected to MongoDB');
	})
	.catch(e => {
	console.log(e);
	});
	
	(...)


![[Pasted image 20221006022124.png]]

<p>실행하면 위와 같이 문구가 출력되면 데이터베이스에 성공적으로 연결된 거십니다.


### 2.4 esm으로 ES 모듈 import/export 문법 사용하기

<p>기존  리액트 프로젝트에서 사용해 오던 ES 모듈 import /export 문법은 Node.js에서 아직 정식으로 지원 되지 않습니다. Node.js에 해당 기능이 구현되어 있기는 하지만 아직 실험적인 단계이기 때문에 기본 옵션으로는 사용 할 수 없으며, 확장자를 .mjs로 사용하고  node를 실행 할때 --experimental-modules라는 옵션을 넣어 주어야 합니다.

<p>Node.js에서 import/export 문법을 꼭 사용해야 할 필요는 없지만, 이 문법을 사용하면 VS Code에서  자동완성을 통해 모듈을 자동으로 쉽게 불러올 수 도있고 코드도 더욱 깔끔해 집니다. 그 래서 우리는 esm이라는 라이브러리의 도음을 받아 해당 문법을 사용해 보겠습니다.

<p>먼저 esm을 yarn으로 설치해주세요.

	yarn add esm

<p> ❗️기존 src/index.js 파일의 이름을 main.js 로 변경하고, index.js 파일을 새로 생성해서 다음코드를 입력하세요.

	// 이 파일에서만 no-global-assign ESLint 옵션을 비활성화합니다.
	/* eslint-disable no-global-assign */
	
	require = require('esm')(module /*, options*/);
	module.exports = require('.main.js');

<p> 다음으로는 package.json 에서 만들었던 스크립트를 조금 수정해 주세요.

![[Pasted image 20221006024101.png]]

<p>ESLint 에서 import/export 구문을 사용해도 오류로 간주하지 않도록 다음과 같이 , eslintrc.json 에서 sourceType 값을 "modules" 로 설정해주세요.

	"parserOptions": {
		"ecmaVersion": "latest"
		"sourceType": "module"
	},

<p> 이제 프로젝트에서 import/export 구문을 자유롭게 사용 할 수 있습니다. 그리고 이전에 만들었던 모듈을 한하나 수정해주 겠습니다.

<p>기존에 실행 중이던 서버는 종료하고, 다시 yarn start:dev 명령어를 입력하여 새로운 스크립트로  서버를 구동하겟습니다.

#### 2.4.2 기존 코드 ES Module 형태로 바꾸기 

<p> 먼저 api/posts/posts.ctrl.js  파일을 열어서 exports 코드를 export const로 모두 변환하세요

	export const write = ctx => { }
	export const list = ctx => { }
	export const read = ctx => { } // 아래도 다 똑같이 exports => export const 로 변경하엿씁니다.

<p>다음으로 src/api/posts/index.jsㅇ을 수정하세요

![[Pasted image 20221006032447.png]]

<p>위의 export const  로 변경하였기떄문에 함수 호출형식을 바꿔보았습니다. 하지만 여기까지 코드작성하면 서버에서 오류가 발생합니다 .이 오류는 파일 두개 더 수정하면 해결이 됩니다..

<p>src/api/index.js

![[Pasted image 20221006032557.png]]


<p>src/main.js

![[Pasted image 20221006032616.png]]

<p> 이제 Postman으로 주소에 요청을 보내 우리가 만든 서버가 오류발생으로 종료되지 않고 잘 작동되는 지 확인할것❗️

![[Pasted image 20221006032718.png]]


<p>코드를 다 작성하고 확인했으면, 마지막으로 프로젝트 루트 디렉터리에 jsconfig.json을 작성하세요

<p>jsconfig.json 

![[Pasted image 20221006032841.png]]

<p>이 파일을 위 코드와 같이 작성해주면 나중에 자동 완성을 통해 모듈을 불러올 수 있습니다. src 디렉터리에 sample.js라는 파일을 작성하고 , api을 입력했을 떄 자동 완성할수 있는인텔리 창이 뜨는지 확인하세요.!

![[Pasted image 20221006033008.png]]



