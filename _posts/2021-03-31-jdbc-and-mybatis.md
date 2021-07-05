---
title:  "JDBC, MyBatis 개념 이해하기"
excerpt: "정확한 개념 쌓기"

categories: SPRING
tags: [JAVA, SPRING, TIL]
---

제대로 이해하지 못하고 사용하고 있는 것들이 많은 것 같아 다시 정리한다.



## JDBC

* Java Database Connectivity

* Java에서 DB 프로그래밍을 하기 위해서 사용되는 API로 자바 웹 어플리케이션과 데이터베이스를 연결 해주는 중간 다리 역할을 해준다. 

  * 자바 어플리케이션 <-> JDBC API <-> JDBC 드라이버 <-> 데이터베이스

    * JDBC 드라이버 ?

      : DBMS와 통신을 담당하는 자바 클래스로 DBMS별로 JDBC 드라이버가 필요함 (jar 파일)

      :memo: Oracle인 경우 Class.forName("oracle.jdbc.driver.OracleDriver") 을 통해서 로딩 할 수 있음

* 데이터베이스 종류에 상관 없이 사용이 가능하다. 

* 관련 패키지

  * `java.sql.Driver`
  * `java.sql.Connection`
  * `java.sql.Statement`
  * `java.sql.ResultSet`
  * 등이 있음. 

* JDBC 연결 순서
  1. JDBC 드라이버를 로드한다.
  2. DriverManager.getConnection() 메서드를 통해서 DB Connection 객체를 얻는다. *= 물리적으로 데이터베이스에 접근한다.*
  3. Connection 객체로부터 쿼리를 수행하기 위한 PreparedStatement 객체를 받는다.
  4. executeQuery를 수행하여 ResultSet 객체를 결과로 받아 데이터를 처리한다. 
  5. 처리 후 사용된 리소스들을 close하여 반환한다.

## Spring JDBC

* JDBC에서 DriverManager가 하던 일들을 JdbcTemplate에 맡긴다 = JDBC API가 하던 일을 JdbcTemplate에게 맡긴다. 

* JDBC Template은 Spring JDBC 접근 방법 중 하나로, 내부적으로 Plain JDBC API를 사용하지만 위와 같은 문제점들을 제거한 형태의 Spring에서 제공하는 class이다.

* 자바 어플리케이션 <-> JDBC Template <-> DataSource <-> JDBC Driver <-> 데이터베이스

  

## DBCP (Connection Pool)

* DB Connection을 보다 효율적으로 사용하고 관리하기 위한 하나의 프레임워크
*  위의 JDBC 연결 순서에서 2번 *DB Connection 객체를 얻는다* 부분이 실질적으로는 가장 시간이 많이 걸리는 부분인데 그 부분을 해결 하기 위함.
*  하나의 Pool을 만들어 두고 Connection 객체를 담아두면 생성해둔 Connection을 가지고 와서 사용 함. 매번 새로운 Connection을 생성하지 않아도 됨. 

#### :balloon:__DataSource__ 

* 자바 어플리케이션과 Connection Pool 사이의 인터페이스로 애플리케이션은 DataSource 객체를 DB Connection의 Factory로 바라보게 된다. 

* commons-dbcp library

* DB Server 기본적인 연결, DB Connection Pooling, 트랜젝션 처리 같은 일을 함

  ``` xml
  <!-- bean 객체 생성 -->
  <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driverClassName}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
  </bean>
  ```

  

:key: Keywords

* DAO: Data Access Object 

  * 데이터베이스의 데이터에 접근하기 위해 생성하는 객체
  * 데이터베이스에 접근하기 위한 로직과 비즈니스 로직을 분리하기 위해 사용됨 = DB에 접속해서 CRUD를 실행하는 클래

* VO: Value Object = read only

  

## MyBatis

* 객체 지향 언어인 자바의 RDMS 프로그래밍을 쉽게 도와주는 프레임 워크로 JDBC 연결을 좀 더 편리하게 사용하기 위함. 
* SQL문을 Mapper 파일로 분리 할 수 있음으로 DAO에서는 아무런 영향을 받지 않고 SQL문을 수정 할 수 있음. 
* MyBatis와 Spring을 연결하기 위해서는 **SqlSessionFactory**가 필요함. DB 연결, SQL 실행, Connection 생성 및 처리를 도와주는 객체임 = Spring에서 SqlSessionFactory를 생성해주기 위해서 SqlSessionFactoryBean 클래스를 설정해줌. 
* DataSource를 이용함으로 DataSource 설정을 해주어야 함. 
  * xml 파일을 통해서 dataSource, SqlSessionFactory, sqlSession을 bean으로 등록함
  * dataSource에는 connection에 필요한 정보 
  * SqlSessionFactory는 SqlSession을 만들기 위한 정보들
  * sqlSession은 직접적인 연결을 위한 사용 객체로 등록한다.



많이 도움을 받은 곳들

https://codevang.tistory.com/249 * 전체적 설명 

https://beaniejoy.tistory.com/24

https://min-it.tistory.com/6

https://jwkim96.tistory.com/62 - mybatis datasource 설정 관련

https://gmlwjd9405.github.io/2018/05/15/setting-for-db-programming.html