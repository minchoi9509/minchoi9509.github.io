---
title:  "Docker란?"
excerpt: "도대체 이해가 안되는 Docker의 세계"

categories: TIL
tags: [Docker, TIL]
---


> 참고
[Docker - 컨테이너란? (Container)](https://captcha.tistory.com/46)  
[컨테이너 및 도커 개념정리](https://velog.io/@geunwoobaek/%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EB%B0%8F-%EB%8F%84%EC%BB%A4-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC)  
[초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)  
[[Docker] docekr-compose란?](https://conanglog.tistory.com/72)  
[10장. 도커파일(Dockerfile)](https://velog.io/@ckstn0777/%EB%8F%84%EC%BB%A4%ED%8C%8C%EC%9D%BCDockerfile)   


## 컨테이너

 애초에 **컨테이너**란?
* 컨테이너 = 기술, 도커 = 컨테이너 기술을 적용한 제품 명, 대표적인 컨테이너 제품
	
* 호스트 OS상에 논리적인 공간(=컨테이너)를 만들어 애플리케이션 작동을 위해 필요한 라이브러리, 애플리케이션을 모아 별도의 서버처럼 작동하게 만든 것

* 서버 가상화 기술

* 개별 소프트웨어의 실행에 필요한 실행 환경을 독립적으로 운용 할 수 있도록 다른 실행 환경과의 간섭을 막고 실행의 독립성을 확보해주는 운영체제 수준의 기술. 

* 가상 머신과의 차이
	* 가상머신(VMware, VirtualBox) : 하드웨어 기반의 게스트 운영 체제로 가상 머신마다 전용 OS가 있기 때문에 CPU 메모리 사용량이 필요 이상으로 많아져 성능 저하가 많음. 
	* 컨테이너: 운영 체제 환경(커널)을 공유하고 별도의 OS가 없기 때문에 성능 저하가 적음. 

* 하나의 서버에 여러개의 컨테이너를 실행하면 서로 영향을 미치지 않고 독립적으로 실행 됨.


## 도커
도대체 정체가 무엇이냐..

* 컨테이너 기반의 오픈소스 가상화 플랫폼

* ES 이미지가 있을거 같아요 > 도대체 **이미지**란 무엇인가? 
	* github library의 개념
	* 컨테이너 실행에 필요한 파일과 설정 값 등을 포함하고 있는 것 = 변하지 않음 
	* 컨테이너는 이미지를 실행한 상태
	* 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 삭제되도 이미지는 변하지 않고 남아 있음 
	* 예를 들어서 저 ES에 대한 이미지를 다운 받고 컨테이너를 생성하게 되면 ES 서버 사용 할 수 있음. 

* 컨테이너 생성 시 레이어 방식을 이용해서 용량을 줄이는 방식 사용

* 최고의 장점이자 회사에서도 도입한 이유는 도커를 이용하면 개발 환경을 빠르게 세팅해서 개발 진행 할 수 있음.
	* 여러 대의 컴퓨터에 동일한 환경을 구축하기 위해서 이전에는 OS 설치, 서버 설치와 같은 과정을 계속 반복 했음
	* 하지만 Docker를 활용하면 같은 이미지를 하나의 컨테이너에 담아 돌리기 때문에 동일한 환경이 바로 구축 됨. 

* 간단 명령어
	* docker -v : 버전 확인
	* docker image ls : 도커 이미지 확인

* Docker Compose란?
	* YAML(yml) 파일  
		* *yaml 파일이란? Json, Xml과 같은 데이터 포맷. 설정 파일 같은 경우에서 많이 사용함 (ex: config.yaml)*
	* Docker Compose 파일을 읽어 들여 모든 서비스들의 생성 및 시작을 하나의 명령어로 시작 할 수 있음. 
	* 클라이언트 웹 서버, 데이터 베이스, Spring 서버, node 서버 등이 모두 필요한 애플리케이션의 경우 한번에 정의 가능
	* 도커 이미지를 사용해서 사용 가능
	* 컨테이너의 실행을 한 번에 관리할 수 있게 해주는 것
	
``` yml
# 버전
version: "3"
# 생성하고자 하는 컨테이너 항목들
services:
  # 컨테이너1
  elasticsearch:
    # 도커 이미지 > 컨테이너가 만들어질 수 있는 기반
    image: docker.elastic.co/elasticsearch/elasticsearch:6.8.9
    # 포트 포워딩 할 포트 지정 <호스트 포트>:<컨테이너 포트>
    # 호스트 포트는 중복될 수 없음
    ports:
      - "5000:5000"
    # <현재 폴더 위치>:<컨테이너 상의 위치>
    volumes:
      - .:/code
    environment:
      FLASK_ENV: development
  # 컨테이너2 : redis
  redis:
    image: "redis:alpine"
```
* Dockerfile 이란?
	* 컨테이너에 설치해야하는 패키지, 소스코드, 명령어, 환경변수 설정 등을 기록한 하나의 파일
	* `Dockerfile` 을 빌드하면 자동으로 도커 이미지가 생성하게 됨
	* 애플리케이션 빌드 및 배포를 자동화 할 수 있음
``` bat
# Dockerfile 빌드 명령어
docker build -t mybuild:0.0 ./
```
* -t 옵션은 생성될 이미지 이름 설정 - Dockerfile 저장 경로 입력