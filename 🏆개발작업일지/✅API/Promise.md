### Promise 란 ?

	여기서 Promise는 자바스크립트 비동기 처리에 사용되는 객체입니다. 여기서 자바스크립트의 비동기 처리란 ‘특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성’을 의미합니다.
	
### Promise 왜 필요한가 ?

	주로 서버에서 받아온 데이터를 화면에 표시할 때 사용합니다. 일반적으로 웹 애플리케이션을 구현할 때 서버에서 데이터를 요청하고 받아오기 위해 아래와 같은 API를 사용합니다.

### Promise의 상태 (State)

	Promise 객체가 생성되고 종료될 때까지는 아래와 같이 3가지 상태(state)를 갖습니다. 여기서 상태란 Promise의 처리 과정(process)을 의미합니다.

	<li>**Pending(대기)** : 비동기 로직 처리의 미완료 상태
	<li>**Fulfilled(이행)** : 비동기 로직 처리가 완료된 상태로 Promise 결괏값 반환 상태
	<li>**Rejected(실패)** :  비동기 로직 처리의 실패 또는 오류 상태

### Pending(대기)

`new Promise`
`new Promise((resole, reject) => {})  //resole 성공 , reject는 실패`  



### 예제

`function increase(number) {

    const promise = new Promise((resolve , reject) => {

        //resolve는 성공 , reject 는 실패

        setTimeout(()=> {

            const result = number + 10;

            if (result > 50){

                //50보다 높으면 에러 발생시키기

                const e = new Error('NumberTooBig');

                return reject(e);

            }

            resolve(result); //number 값에 +10 후 성공 처리.

        },1000);

    });

    return promise;

}

increase(0)

    .then(number => {

        //Promise에서 resolve된 값은 .then을 통해 받아 올 수 있음

        console.log(number)

        return increase(number); //Promise을 리턴하면

    })//10

    .then(number => {

        //또 .then으로 처리 가능

        console.log(number);

        return increase(number);

    })//20

    .then(number => {

        console.log(number);

        return increase(number);

    })//30

    .then(number =>{

        console.log(number);

        return increase(number);

    })//40

    .then(number =>{

        console.log(number);

        return increase(number);

    })//50

    .catch(e => {

        // 도충에 에러가 발생한다면 .catch를 통해 알 수 있음

        console.log(e); //Error

    })`
  

### 결과

	>10
	>20
	>30
	>40
	>50
	>NumberTooBig

   
여러 작업을 연달아 처리한다고 해서 함수를 여러 번 감싸는 것이 아니라 .then을 사용하여 그다음 작업을 설정하기 떄문에 콜백 지옥이 형성되지않는다.!