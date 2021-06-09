---
title:  "모던 JavaScript"
excerpt: "https://ko.javascript.info/"

categories: Javascript
tags: [Javascript, TIL]

---

기억하고 싶은 부분 메모

* 스크립트를 별도의 파일에 작성하면 브라우저가 스크립트를 다운받아 캐시(cache)에 저장하기 때문에, 성능상의 이점 존재. 여러 페이지에서 동일한 스크립트를 사용하는 경우, 
브라우저는 페이지가 바뀔 때마다 스크립트를 새로 다운받지 않고 캐시로부터 스크립트를 가져와 사용합니다. 스크립트 파일을 한 번만 다운.

* 세미콜론은 생략할 수 있습니다. 하지만 세미콜론을 사용하는 것이 더 안전함.

* __use strict__
  * 스크립트 최상단 혹은 함수 본문 맨 앞에 위치하여 스크립트 전체를 __모던한__ 방식으로 동작 시킴
  * 개발한 기능을 테스트하기 위해 브라우저 콘솔을 사용하는 경우 기본적으로 적용되어 있지 않음.
  * 모던 자바스크립트에서 제공하는 클래스, 모듈이라는 구조를 이용하면 `use strict`가 자동으로 적용되어 스크립트에 붙일 필요 없음.
 
* 함수형 언어 (ex: 스칼라, 얼랭)
  * 변수에 값이 저장되면 그 값을 영원히 유지하고 다른 값을 저장하기 위해서는 새로운 변수를 선언 해야 함. 
 
* let (변수) <-> const (상수)
 
* BigInt 
  * [관련설명] (https://ko.javascript.info/bigintf)
  * 내부 표현 방식 때문에 자바스크립트에선 (253-1)(9007199254740991) 보다 큰 값 혹은 -(253-1) 보다 작은 정수는 '숫자형’을 사용해 나타낼 수 없습니다.
  * BigInt형 값은 정수 리터럴 끝에 n을 붙이면 만들 수 있습니다.
  
* undefined <-> null
  * undefined : 변수는 선언했지만 값이 할당되지 않은 상태.
  * null : 존재하지 않는 값 (nothing), 비어있는(empty) 값, 알수 없는(unknown) 값.
 
* typeof
  ``` javascript
  typeof Math // "object"  (1)

  typeof null // "object"  (2)

  typeof alert // "function"  (3)
  ```
  * object - 복잡한 데이터 구조를 표현 할 때 사용 
  * "function" 함수형은 따로 데이터형이 아님. 객체형에 속하는 type.

 
