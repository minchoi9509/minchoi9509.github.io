---
title:  "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"
excerpt: "프로젝트 환경 설정 ~ 스프링 웹 개발 기초"

categories: SPRING
tags: [JAVA, SPRING, TIL, 인프런]

---





도저히 Spring을 알고 쓰고 있다는 생각이 안들어서 듣는다.

강의 듣기 전 `1. Java 11 버전 업그레이드` `2. InteliJ 다운로드` 를 진행했다. 



#### 프로젝트 생성하기

---

* 스프링 부트 스타터 사이트를 통해 스프링 프로젝트 생성

  `Spring initializr` https://start.spring.io/ : Spring boot 기반 프로젝트를 생성해주는 사이트

  :white_check_mark: **Project**

  * Maven Project <-> **Gradle Project**
  * 필요한 라이브러리를 가져오고 빌드까지 도와주는 tool
  * 과거에는 Maven을 많이 사용했지만 요즘 추세는 Gradle을 많이 사용함. 

  

  :white_check_mark: **Spring Boot**

  * 버전

  

  :white_check_mark: **Project Metadata**

  * Group : 보통 도메인
  * Artifact: 빌드 결과물 (프로젝트명)
  * Name, Desc.

  

  :white_check_mark: **Dependency**

  * 프로젝트 시작시 어떤 라이브러리를 이용할 건지 체크
  * Spring web
  * thymeleaf : 회사마다 다르게 사용



* 설정이 끝나면 압축 파일 생성
* `build.gradle` 버튼을 선택해서 오픈함
* 프로젝트 생성과 동시에 외부 라이브러리를 다운 받음 



#### 프로젝트 구조

---

src

ㄴ main

​	ㄴ java

​	ㄴ resources : 실제 Java 코드 파일을 제외한 xml, propertise, html과 같은 다른 파일

ㄴ test

* main과 test 폴더가 나눠져 있는 것이 학계 정설
* test 코드는 매우 중요하다.



:bulb: build.gradle 파일

* gradle을 통해서 버전 설정하고 라이브러리를 가지고 옴
* repositories { mavenCentral() } : 앞선 라이브러리를 이 url에서 다운로드 받아라
* dependecies: 앞에서 설정했던 친구들



* Spring boot에서 톰캣이라는 웹 서버를 내장하고 있어서 한번에 자동으로 띄움 (충격진실)



#### 라이브러리 살펴보기

---

* dependecies 외에도 내가 설정해주지 않은 친구들도 알아서 가지고 옴. 

* gradle이나 maven과 같은 툴들이 의존 관계를 관리해줌.

  * ex: Spring-boot-starter-web 과 같은 라이브러리를 다운 받으면 필요한 다른 라이브러리 (ex: 웹 mvc, 톰캣...)들을 땡겨옴
  * 웹 서버(WAS) 라이브러리를 이용 (충격진실)

* spring-boot-starter(공통)

  * spring-boot

    * spring-core

  * spring-boot-starter-logging

    * logback, slf4j

    

#### 빌드하고 실행하기

---

:bulb: 윈도우 기준

* `gradlew.bat build` 명령어를 통해서 빌드 실행
* `./build/libs` 폴더에서 java -jar 명령어를 이용해서 해당 jar 파일을 실행 할 수 있음
* `./gradlew.batch clean`을 통해서 build 폴더 clean 한 뒤에 다시 가능. 

이 jar 파일만 가지고가서 실행시키면 충격적이게도 빌드 완료





#### 스프링 웹 개발 기초

---

웹을 개발하는 방법은 크게 3가지가 있음

1. 정적 컨텐츠 2. MVC와 템플릿 엔진 3. API



* 정적 컨텐츠 

  * 파일을 바로 웹 브라우저에 뿌려주는 것

  * `static` 폴더 하위에 파일(hello-static.html)을 만들어주면 됨.

  * 웹 브라우저 -> 내장 톰캣 서버 -> hello-static 관련 컨트롤러를 스프링 컨테이너에서 확인 -> 없다면 static 폴더 하위를 뒤짐

    

* __MVC와 템플릿 엔진__

  * 가장 많이 쓰임

  * JSP, PHP와 같은 것이 템플릿 엔진 (서버에서 약간의 변동)

  *  Model, View, Controller  (MVC - model2 방식) : 서로의 역할에 집중 할 줄 알아야 함. 

    * View : 화면을 그리는데 집중
    * Model, Controller: 비즈니스 로직에 집중

  * 웹 브라우저 -> 

    내장 톰캣 서버 -> 

    Controller를 뒤져보다가 "hello-mvc" 를 가진 메소드를 호출 -> 

    `return: hello-template model(name:spring)` 값을 Return 해줌 -> 

    Spring 에게 return 하면 __viewResolver__를 통해서 templates/hello-template.html 파일을 찾아서 Thymeleaf 템플릿 엔진에서 처리 해준다. 



* __API__

  * VUE, React

  * 서버끼리 통신 하는 경우

  * @ResponseBody

    * HTTP에서 header부, body부가 있는데 응답 body 부분에 데이터를 직접 넣어 주겠다는 의미.
    * View 형식 (HTML)을 통하는 형식이 아니라 그냥 진짜 데이터 자체를 전달해줌

  * 객체를 반환하면 Json 방식으로 데이터를 Return 해 줌

  * 웹 브라우저 -> 

    내장 톰캣 서버 -> 

    Controller를 뒤져보다가 `@ResponseBody` 라는 어노테이션이 붙은 메소드를 확인하게 되면 ->  

    Return 타입에 따라 1. String (데이터 자체 반환) 2. 객체 (조건에 따라서 기본적으로는 Json으로 반환)  __HttpMessageConverter__ 를 통해서 (1. StringHttpMessageConverter 2. MappingJackson2HttpMessageConverter)  데이터 변환 뒤 Http Body 부분에 데이터를 집어 넣어서 Return 해줌 

 



