---
title:  "객체 지향 모델링"
excerpt: "자바 객체지향 디자인 패턴"

categories: JAVA
tags: [JAVA, 디자인패턴, TIL]
---

처음 구조를 짤 때 한계를 많이 느껴서 공부 해 본다. 


## 객체지향 모델링

``` java
public class Person {
	public Phone[] phones;

	public Person() {
		phones = new Phone[2];
	}	
}
```

사람과 핸드폰 관계에서 사람은 핸드폰을 가지고 있으므로 생성자 안에서 정의 할 수 있다. 
사람이 가지고 있는 핸드폰이 집 전화, 사무실 전화로 두가지 종류를 가지고 있는 경우 저런 식으로 배열로 저장하면 인덱스를 통해 구분해야 함으로 불편하다.


``` java
public class Person {
	public Phone homePhone;
	public Phone officePhone;


	public Person() {
		homePhone = new Phone("home");
		officePhone = new Phone("office");
	}	
}
```

이런 식으로 생각했는데 책에서는 getter, setter를 요 안에서 이용해서 사용 하고 있었다. 
하지만 Person 클래스 안에서 phone에 대한 것을 set하는게 맞는걸까 라는 생각으로 나는 반대..


``` java
public class Student {
	public String name;
	public List<Course> courses;

	public Student(String name) {
		this.name = name;
		this.courses = new List<Course>();
	}

	public void registerCourse(Cousre course) {
		courses.add(course);
	}

	public void dropCourse(Course course) {
		courses.remove(course);
	}
}

public class Cousre {
	public String name;
	public List<Student> students;

	public Cousre(String name) {
		this.name = name;
		this.students = new List<Student>();
	}

	public String getName() {
		return name;
	}
}
```

하지만 이 관계는 정말 이상한.. 학생들만 리스트로 강의 목록을 가지고 있는 관계이므로 Course 클래스를 변경해 보았다.

``` java
public class Cousre {
	public String name;
	public List<Student> students;

	public Cousre(String name) {
		this.name 
	}

	public String getName() {
		return name;
	}
}
```

이 정도로만 생각했었는데 이거 외에도 Course에 student가 추가가 되어야 하니까 addStudent(Student student) 메서드를 통해서 학생을 강의에 추가하는 기능도 필요하고, 제거하는 기능도 필요하고 (왜냐면 학생 쪽에도 강의에 똑같은게 있고 course List와 student List는 어떻게 보면 일치되어야 하니까) get에 대한 부분도 생각하지 못했다.


## 객체지향 원리

	* 응집도 : 클래스나 모듈 안의 요소들이 얼마나 밀접하게 관련되어 있는가?
	* 결합도 : 어떤 기능을 실행하는데 있어 다르 클래스나 모듈들에 얼마나 의존적인지를 나타냄
	* 높은 응집도와 낮은 결합도를 유지할 수 있도록 설계 필요

* 추상화
	* 구체적인 사물들의 공통적인 특징을 파악하여 하나의 개념으로 다루는 수단

* 캡슐화
	* 낮은 결합도 유지 할 수 있는 설계 원리
	* 정보 은닉을 통해 높은 응집도와 낮은 결합도를 갖도록 함

* 일반화 관계
	* 일반적으로 __상속 관계__를 의미함. 하지만 이 표현은 기능의 재사용성에만 신경을 쓴 한정된 시각임.
	* 자식 클래스를 외부로부터 은닉하는 캡슐화의 일종
	* 하지만 재사용!에만 집중을 했다가 불필요한 속성이나 연산들도 상속을 받게 된다
		* 어떤 클래스의 일부 기능만 재사용하고 있는 경우에는 그 부분은 다르클래스의 객체가 기능을 실행 할 수 있도록 함. 

* 다형성
	* 서로 다른 클래스의 객체가 같은 메세지를 받았을 때 각자의 방식으로 동작하는 능력

* 피터 코드의 상속 규칙 : `사람 < 운전자, 회사원`
	* 자식 클래스와 부모 클래스 사이에는 __역할 수행 관계가 아니어야 한다__ > 어떤 사람이 운전자 역할, 회사원 역할을 할 수 있는 것이어서 상속 관계로 표현 되면 안됨.
		* ? 사실 이해가 잘 안된다. 회사원 extends 사람이 안된다... 왜냐면 
	* 한 클래스의 인스턴스는 __다른 서브 클래스의 객체로 변환할 필요가 없어야 한다__ > 자식 클래스 인스턴스 사이에 변환 관계가 필요한가? 운전자가 회사원일 수도, 회사원이 운전자일 수도 있으므로 규칙 위배.
	* 자식 클래스가 부모 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행해야 한다.
	* 자식 클래스가 단지 일부 기능을 재사용할 목적으로 유틸리티 역할을 수행하는 클래스를 상속하지 않아야 한다.
	* 자식 클래스가 역할, 트랜잭션, 디바이스등을 특수화 해야 한다. 


## SOLID

* SRP (Single Responsibility Principle) : 단일 책임 원칙
	* 기본 단위 : 객체
	* 예측하지 못한 변경사항이 발생하더라도 유연하고 확장성이 있도록 시스템 구조 설계 필요

* AOP (Aspect-Oriented Programming) 
	* 기존 코드를 변경하지 않고도 시스템 핵심 기능에서 필요한 부가 기능을 효과적으로 이용 할 수 있음

* OCP (Open Closed Principle) : 개방 폐쇄 원칙
	* 기존 코드 변경하지 않으면서 기능 축가 할 수 있도록 설계 
	* 무엇이 변하는 것인지 무엇이 변하지 않는 것인지 구분 필요
	* 단위 테스트 설계 하는 경우
		* test type 변수로 주는 경우에 if-else 지옥에 빠지게 하지 말고 test type set 할 수 있는 메서드 만들어 주면서 테스트 효율성 높임 

* LSP (Liskov Substitution Principle)
	* 부모 클래스의 메서드를 __자식 클래스에서 재정의 하지 않는다.__
	*  자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행 할 수 있어야 함 

* DIP (dependency Inversion Principle) 
	* 거의 변화가 없는 것(=정책, 전략, 큰 흐름, 개념과 같은 추상적인 것)에 의존하는 원칙 
	* 어떤 클래스가 도움을 받을 때 (=이용 할 때) 구체적인 인스턴스보다는 인터페이스, 추상 클래스와 같이 큰 개념 자체와 의존 관계를 맺을 수 있도록 설계 해야 한다. 

* ISP (Interface Segregation Principle) 
	*  인터페이스 분리 원칙 : 자신이 이용하지 않는 기능에는 영향을 받지 않아야 함

## 디자인 패턴
> 바퀴를 다시 발명하지 마라

* GoF 
	* 생성 패턴
		* 객체 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 받지 않도록 유연성 제공
		* ex) 추상 팩토리, 빌더, 팩토리, 프로토타입, 싱글턴
		
	* 구조 패턴
		* 클래스나 객체를 조합해서 더 큰 구조를 만드는 패턴
		* ex) 어댑터, 퍼사드, 브리지, 컴퍼지트, 플라이웨이트
		
	* 행위 패턴
		* 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
		* 객체 작업 분리
		* ex) 커맨드, 인터프리터, 이터레이터, 미디에이터, 메멘토, 옵저버, 스테이트, 스트래티지, 비지터 

