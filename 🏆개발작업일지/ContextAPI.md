# ContextAPI

## ContextAPI 란 ? 
**리액트 프로젝트에서 전역적으로 사용할 데이터가 있을 때 유용한 기능**
ex) 리덕스, React-Router , styled-components 등의 라이브러리는 ContextAPI 기반으로 구현

## ContextAPI 실습 흐름 
<li>ContextAPI를 사용한 전역 상태 관리 흐름 이해하기
<li>기본적인 사용법 익히기 
<li> 동적 Context 사용하기
<li> Consumer 대신 Hook 또는 static contextType 사용하기


### ContextAPI를 사용한 전역 상태 관리 흐름 이해하기
![[Pasted image 20220815040053.png]]

리액트 애플리케이션은 컴포넌트 간에 데이터를 props로 전달하기 때문에 컴포넌트 여기저기서 필요한 데이터가 있을 때는 주로 최상위 컴포넌트인 App의 state에 넣어서 관리합니다.

	위의 사진을 보시면 root 컴포넌트 가 가지고있는 예로들어 value 값을 F 컴포넌트와 J 컴포넌트에 전달하려면 여러 컴포넌트를 거쳐야한다 

		F의 경우 Root -> A  -> C -> F 의 흐름이고 , J의 경우 Root -> H -> J 의 흐릅니다.

실제 리액트 프로젝트에서는 더 많은 컴포넌트를 거쳐야 할 때도 있고 다루어야 하는 데이터가 휠씬 많아질 수도 있으므로 , 이런 방식을 사용하려면 유지 보수성이 낮아질 가능성이 있다.

그렇기 때문에 리더스나 MobX 같은 상태 관리 라이브러리를 사용하여 전역 상태 관리 작업을 더 편하게 처리하기도 한다.


