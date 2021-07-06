---
title: "자바스크립트 비동기 처리와 콜백"
excerpt: "Node.js 사용 하기 전 기초부터 탄탄히"
categories: Javascript
tags: [Javascript, TIL]
---
> 참고 : [자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)    

## 비동기 처리

![](https://i.imgur.com/hh3Mawr.png)

* 자바스크립트는 하나의 작업이 끝날 때 까지 기다리는 동기적 처리 방식이 아닌 흐름을 멈추지 않고 **여러가지 작업을 동시에 작업하는 비동기적 처리 방식**의 특성을 가짐

``` javascript
// Ajax
function getData() {
	let data;
	$.get('https://localhost:8080/example', function(response) {
		data = response;
	}); 
	return data;
}

console.log('data', data); // data: undefined
```
* 통신 과정을 기다리지 않고 data를 console로 찍었기 때문에 undefined가 찍히게 됨.

``` javascript
function getData(callback) {
	$.get('https://localhost:8080/example', function(response) {
		callbaback(response);
	});
}

getData(function(data) {
	console.log(data);
});
```

* 비동기 처리 로직 사용 시 콜백 지옥에 빠지지 않도록 조심해야 함.

## Promise

* `Promise`는 자바스크립트 비동기 처리에 사용되는 개체
* 서버에서 받아온 데이터를 화면에 표시할 때 사용 
* Promise의 3가지 states
	* Pending(대기) : 비동기 처리 로직이 아직 완료되지 않음
	* Fulfilled(이행) : 비동기 처리가 완료되어 Promise가 결과 값을 반환한 상태
	* Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태
 
* Promise API를 사용한 구조 
``` javascript
function geteData(callback) {
	// new Promise() 추가 -- 대기 상태
	// resolve, reject 인자와 함께 콜백 함수 선언 
	return new Promise(function(resolve, reject) { 
		$.get('https://localhost:8080/example', function(reponse) {
			// 데이터를 받으면 resolve() 호출 -- 이행 상태
			if (response) {
				resolve(response);
			}
			reject(new Error("Request is failted")); // 실패 상태
		});
	});
}

// 성공
getData().then(function(data) {
	console.log(data);
});
// 실패한 경우 - exception에 대해서 catch()로 받을 수 있음
getData().then().catch(function(err) {
	console.log(err);
});
```

## async와 await

* 비동기 처리 패턴 중 가장 최근에 나온 문법
``` javascript
async function getData() {
	const user = await getUserData();
	if (user.id === 1) {
		console.log(user.name);
	}
}
```