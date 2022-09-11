# API 

### API 작업일지

  

이번 14장에서는 카테고리별로 최신 뉴스 목록을 보여 주는 뉴스 뷰어 프로젝트를 진행해 보겠습니다.

  

이번 실습 흐름은


<li>비동기 작업의 이해

<li>axios로 Api 호출해서 데이터 받아오기

<li>newsapi Api 키 발급받기

<li>뉴스 뷰어 UI 만들기

<li>데이터 연동하기

<li>카테고리 기능 구현하기

<li>리액트 라우터 적용하기


### 1. 비동기 작업의 이해

웹 애플리케이션을 만들다 보면 처리할 때 시간이 걸리는 작업이 있다.

예를 들어 웹 애플리케이션에서 서버 쪽 데이터가 필요할 때 Ajax 기법을 사용하여 서버의 API를 데이터를 수신한다.

이렇게 서버에 API를 사용해야 할 때는 네트워크 송수신 과정에서 시간이 걸리기 떄문에 작업이 즉시 처리되는 것이 아니라, 응답을 받을 때까지 기다렸다가 전달받은 응답 데이터를 처리!! 
이과정에서 해당 작업을 비동기적으로 처리한다.


### 2. axios로 Api 호출해서 데이터 받아오기 

일단 라이브러리 설치를 해준다
	`yarn create react-app news-viewer`
	`cd news-viewer`
	`yarn add axios`

### 3. newsapi API 키 발급받기

이번 프로젝트에서는 newsapi에서 API를 사용해서 최신 뉴스를 불러온 후 보여줄것입니다.

아래 사이트에서 가입후 API주소를 받아온다
`https://newsapi.org/s/south-korea-news-api` 

![[Pasted image 20220812211537.png]] 그후 App.js에 작성해준다

**결과**

![[Pasted image 20220812211628.png]]



### 4. 뉴스 뷰어 UI 만들기

	src 디렉터리 안에 components 디렉터리를 생성한뒤 NewsItem.jsx 과 NewsList.jsx 파일을 생성한다

<li>NewsItem  : 각 뉴스 정보를 보여 주는 컴포넌트
<li>NewsList : API를 요청하고 뉴스 데이터가 들어 있는 배열을 컴포넌트 배열로 변환하여 렌더링해주는 컴포넌트 

### 4.1 NewItem 만들기

	각 뉴스 데이터가 지니고 있는 정보로 이루어진 JSON 객체입니다. 그중에서 다음 필드를 리애트 컴포넌트로 나타내겠습니다.

<li>**title** : 제목
<li>**description** : 내용
<li>**url** : 링크 
<li>** urlToImage : 뉴스 이미지