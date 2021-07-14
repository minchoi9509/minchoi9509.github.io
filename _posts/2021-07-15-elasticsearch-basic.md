---
title: "Elasticsearch DSL Query"
excerpt: "익숙하지 않은 ES와 친해지기"
categories: Elasticsearch
tags: [Elasticsearch, TIL]
---
> 참고: [filed vs field.keyword](https://blog.voidmainvoid.net/314)   

* Elasticsearch란?
	* 오픈소스 분산 검색엔진
	* 실시간 검색 시스템 > 클러스터가 실행되는 동안 계속해서 데이터가 입력됨 
	* Json 문서 기반으로 질의에 사용되는 쿼리문과 쿼리에 대한 결과 모두가 Json 형태로 되어 있음
	* Rest API를 기본으로 지원하여 모든 데이터의 CRUD를 Http 프로토콜 메소드를 이용하여 처리 한다. 
	* 클러스터에 여러 Elasticsearch 노드가 있는 경우 저장된 문서가 클러스터 전체에 분산되어 다시 저장됨.
	* Json 스타일 쿼리 언어 **(Query DSL)**을 사용하여 모든 검색 기능에 액세스 가능하고 SQL 스타일 쿼리도 구성 가능하여 JDBC, ODBC 드라이버를 사용하여 SQL을 통해 검색이 가능함.
	* Aggregation(집계)를 통하여 데이터 분석을 할 수 있다. 이 때 Aggregation의 경우 검색 할 때와 같은 데이터 구조를 활용함으로 속도가 빠르고 실시간으로 데이터를 분석하고 시각화 할 수 있다. 
	* relevancy = 정확도 = 정확돋를 기반으로 사용자가 가장 원하는 결과를 먼저 보여 줄 수 있음 
	
* ELK
	* Logstash : 실시간 파이프라인 기능을 가진 오픈소스 데이터 수집 엔진
	* Elasticsearch : Logstash로부터 받은 데이터를 검색 및 집계하여 필요한 관심있는 정보 획득
	* Kibana : Elasticsearch의 검색을 통해 데이터를 시각화 및 모니터링 할 수 있는 도구

* Elastic index
	* index : RDBMS에서 데이터베이스, 도큐먼트 모음
	* 인덱스는 하나의 노드에만 존재하지 않고 샤드 단위로 구분되어서 여러 노드로 나뉘어서 저장됨. 
	* 정보 단위 : settings, mappings
	* 인덱스에 도큐먼트를 새로 추가하면 자동으로 도큐먼트들에 대한 mapping이 이뤄짐으로 미리 인덱스 템플릿을 정의해두면 정의 해 놓은 매핑에 맞추어 데이터가 입력됨. 

* Elastic index template
	* 인덱스의 mapping은 template을 사용해서 생성되도록 설정하면 편리함. 
	* 인덱스 패턴이 `test-*` 이라면 test-min, test-test, test-log 와 같은 모든 인덱스들이 해당 템플릿이 적용된다.

* Elastic DSL Query
	* 풀 텍스트 쿼리  (Full Text Query)
		* match _all
		* match : 가장 일반적인 쿼리
	* Bool Query
		* 여러 쿼리를 조합하기 위해서 상위에 bool 쿼리를 사용하고 그 안에 다른 쿼리들을 넣는 식으로 사용 가능
		* must : 참인 도큐먼트 검색
		* must_not : 거짓인 도큐먼트 검색
		* should : 검색 결과 중 이 쿼리에 해당하는 도큐먼트의 점수를 높임
		* filter : 참인 도큐먼트를 검색하지만 스코어를 계산하지 않고 must보다 검색 속도가 빠르고 캐싱이 가능함. > 검색어로 정확도가 높은 상품명을 검색하면서 생산 업체를 다시 필터링하는 등의 용도로 사용 가능 
	* 범위 쿼리 (Range Query)
	* Term 쿼리 
		* 검색어 그대로! 일치하는 것을 찾음

* 역 인덱스 
	* 풀 텍스트 검색을 하기 위해서는 데이터를 검색에 맞게 가공하는 작업이 필요함 > Elasticsearch의 경우 데이터를 저장하는 과정에서 이 작업을 처리함
	* 역 인덱스 (Inverted index) = 주요 키워드에 대한 내용이 몇 페이지에 있는지 찾아볼 수 있는 페이지

* text vs keyword
	* 검색 쿼리 만들어보고 있었는데 `id` key 값을 사용하려고 했는데 `id.keyword` 로 변경해서 사용하라는 에러가 떴었음
	* keyword : 집계 또는 정렬에 사용할 필드를 보통 keyword 타입으로 지정함
