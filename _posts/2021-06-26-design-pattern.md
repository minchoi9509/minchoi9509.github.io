---
title:  "스트래티지 패턴, 싱글턴 패턴, 스테이트 패턴, 커맨드 패턴, 옵저버 패턴, 데커레이터 패턴"
excerpt: "자바 객체지향 디자인 패턴"

categories: JAVA
tags: [JAVA, 디자인패턴, TIL]
---

## 스트래티지 패턴

> 요즘은 안 하지만 열심히 했던 쿠키런 킹덤을 예를 들어보면
> 
> 엄청 많은 종류들의 쿠키들이 존재한는데 기본적으로 쿠키들은 1. 공격한다. 2. 패시브를 사용한다. 라는 공통점을 가지고 있음. 이 구조를 설계해 보자.

-   만약 추상 클래스를 상속 받는 형식으로 구현한다면?
    
    1.  만약 허브맛 쿠키의 패시브를 변경하고 싶은 경우 기존에 구현되어 있던 메서드를 수정해야 한다.
        
    2.  허브맛 쿠키의 `공격한다`라는 기능을 다크 초코맛 쿠키의 `공격한다`라는 기능과 동일하게 변경하는 경우 동일한 기능을 실행하는 중복 메서드를 구현하게 된다.
        
    3.  새로운 신규 쿠키를 구현해야 하는 경우에 캡슐화 단위이므로 단지 쿠키 종류를 추가하는 것은 쉽지만 새로운 쿠키에 공격 및 패시브 방법을 추가하거나 변경 할 때 중복 코드가 발생 할 수 있음. 그리고 이 중복된 코드를 변경해야 하는 경우에는 모든 쿠키에게 가서 변경 해줘야 하는 번거로움이 생김.
        
-   그렇다면 어떻게?
    
    1.  **무엇이 변화해서** 문제가 일어나고 있는지 파악한다. 지금과 같은 경우에는 공격 방식 또는 패시브가 변하는 경우에 따라 문제가 일어난다는 것을 알 수 있다.
        
    2.  공격 방식 / 패시브에 대한 인터페이스를 각각 만들고 이를 구현한 클래스를 만든다.
        

-   스트래티지 패턴은 **전략(=일 수행 방식, 비즈니스 규칙, 문제를 해결한느 알고리즘)을 쉽게 바꿀 수 있도록** 함.

## 싱글턴 패턴
* 단 하나의 객체를 생성해 이를 어디에서든지 참조할 수 있게 하는 것

* 정적 변수, 메서드를 이용해서 Printer(싱글턴 객체) 생성 하는 방법
	``` java
	public Class Printer {
		private static Printer printer = null;
		pricate Printer() {}
		
		public static Printer getPrinter() {
			if (printer == null) {
				printer = new Printer(); // Printer 인스턴스 생성
			}
			return printner;
		}
	}
	```
	* 문제점
		* 단일 스레드일 때는 문제가 없지만 다중 스레드에서 Printer을 사용 할 때 인스턴스 생성 시 문제 발생 > 이 문제 MongoDB 커넥션 풀 생성 할 때 겪었던 문제.  내 나름의 싱글턴 패턴을 구현했던 거라고 생각 했었는데 커넥션 풀이 여러개 생성 되는것 같은 기억이..

	* 해결책
		1. 정적 변수에 인스턴스를 만들어 바로 초기화하는 방법
			`printer` 변수를 null로 초기화하지 않고 인스턴스 생성 후에 사용한다. `priate static Printer printer = new Printer();`
		2. 인스턴스를 만드는 메서드에 동기화 하는 방법
			* `public synchronized static Printer getPrinter()` 
			* 왠지 이 방법을 쓰는 걸 본거 같은 이 기분..?

* 싱글턴 패턴을 굳이 사용하지 않고 정적 메서드로만 이루어진 정적 클래스를 사용해도 동일한 효과를 얻을 수 있음. 

## 스테이트 패턴
* `상태` : 객체가 시스템에 존재하는 동안 (=객체의 라이프 타임 동안) 객체가 가질 수 있는 어떤 조건이나 상황
* if-else문으로 도배 되어 새로운 상태 변화가 존재하는 경우 모든 경우에 따라 코드 수정해야 하는 경우를 방지한다. 
* **상태를 클래스로 분리해 캡슐화**하고 상태에 의존적인 행위들도 상태 클래스에 둔다.

1. Light ON 버튼을 한번 누르면 켜진다.
2. ON 상태의 버튼을 한번 더 누르면 취침 상태로 변한다.
3. 취침 상태의 Light를 한번 더 누르면 OFF 상태로 변한다.
4. OFF 상태의 버튼을 누르면 ON 상태로 변한다.

``` java
	public class Light {
		private State state;
		
		public Light() {
			state = new OFF();
		}
	
		public void setState(State state) {
			this.state = state;
		}
	
		public void clickOnButton() {
			state.clickOnButton(this);
		}
		
		public void clickOffButton() {
			state.clickOffButton(this);
		}
	}

	public interface LightState {
		public void clickOnButton(Light light);
		public void clickOffButton(Light light);
	}

	public class ON implements LightState {
		// 구현이 잘 되지 않았던 부분 > getInstance를 통해서 상태 넘길 생각을 못 했음..
		private static LightState instance = new ON();
		
		public LightState getInstance() {
			return instance;
		}
		
		@Override
		public void clickOnButton(Light light) {
			light.setState(SLEEPING.getInstance());
			System.out.println("SLEEPING");
		}

		@Override
		public void clickOffButton(Light light) {
			light.setState(OFF.getInstance);
			System.out.println("OFF");
		}
	}
	
	// OFF와 SLEEPING 클래스도 동일한 구조
```

* 스테이트 패턴 보면서 뭔가 모르게 스트레티지 패턴이랑 비슷하다는 느낌을 계속 받았다. 클래스를 계속 쪼개는 부분에서 비슷하게 느껴진 것 같다. 


## 커맨드 패턴
* 실행될 기능을 캡슐화해 메서드에서 호출함 = 클래스 내부에서는 캡슐화 된 동일한 메서드 호출. 
* 이벤트가 발생했을 때 실행될 기능이 다양하면서도 변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 유용함.

## 옵저버 패턴 
참고: [[디자인패턴] 옵저버 패턴 (Observer Pattern) 아주 간단하게 정리해보기  ](https://pjh3749.tistory.com/266)  

* 데이터의 변경이 발생했을 경우 상대 클래스나 객체에 의존하지 않으면서 **데이터 변경을 통보하는 경우**에 사용
* 객체 상태가 변화 (=데이터 변경, 상태 변경) 했을 때 그와 연결된 객체들에게 해당 변화에 대한 알림을 보내는 디자인 패턴
* 사용 예시
	* 차량의 연료가 소진될 때까지의 주행 가능 거리를 출력하는 클래스, 연료량이 부족하면 경고 메세지를 보내는 클래스
	* 분산 이벤트 핸들링 시스템
	* 발행/구독 모델 
* Java에서도 옵저버 클래스`Observable` 를 제공 함 

## 데커레이터 패턴
* **기본 기능에 추가할 수 있는 기능의 종류가 많은 경우**에 각 추가 기능을 Decorator 클래스로 정의한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계하는 방식
* 기본 기능에 추가하는 것을 상속(extends)를 통해서도 사용 할 수 있지만 그렇게 되면 추가 기능들을 조합해서 사용할 때 불편함을 겪음으로 데커레이터 패턴을 사용함. 