---
title:  "팩토리 패턴"
excerpt: "팩토리 패턴 이해하기"

categories: JAVA
tags: [JAVA, 디자인패턴, TIL]
---

이번 프로젝트에서는 백엔드 쪽을 맡게 되었는데 코드 대체적으로 팩토리 패턴이 많이 보여서 왜 이렇게 생겼나 하고 공부해본다. 
이해를 먼저하고 천천히 내용을 업데이트한다. 


객체지향 디자인패턴 원칙: 확장에 대해서는 열려있고 변화에 대해서는 닫혀있어야 한다. 바뀔 수 있는 부분을 찾아 바뀌지 않는 부분과는 분리시켜 코드 유연성을 높여야한다. 



### 팩토리 패턴 (Factory Pattern)

* 객체를 생성하기 위해서 인터페이스를 정의하는데 어떤 클래스의 인스턴스를 만들지는 __서브 클래스__에서 결정하게 하는 것
* 객체지향 성질 중 하나인 __다형성__을 이용하여 의존성, 결합도를 낮아주게 함. 
* 객체를 생성하는 코드를 많이 사용하는데 공통적인 부분은 있지만 성격에 따라 다른 유형의 객체를 생성하고 싶을 때 많이 사용하는 패턴.



요즘 열심히 하고 있는 쿠키런의 쿠키들을 통해서 생각해보면

```java
Cookie drawCookie(String type) 
{
	Cookie cookie;
	
    // 변할 수 있는 부분
    if (type.equals("dealer")) 
    {
        cookie = new Espresso();
    } else if (type.equals("healer"))
    {
        cookie = new Herb();
    } else if (type.equals("tanker"))
    {
        cookie = new darckChocolate();
    }
    
    // 변하지 않는 부분
    cookie.prepare();
    cookie.bake();
    cookie.cut();
    
    return cookie;

}
```

쿠키의 type들은 보통은 저렇게 있긴하지만 추가 될 수도 있고 혹시 없어질 수도 있다. 

type에 따라서 쿠키들을 만드는 공장(Factory)를 만들어서 사용 할 수 있다. 



이것은 간단한 Factory를 만든 예제이다. 

``` java
public class SimpleCookieFactory {
    public Cookie createCookie(String type) {
        Cookie cookie = null
            
         if (type.equals("dealer")) 
        {
            cookie = new Espresso();
        } else if (type.equals("healer"))
        {
            cookie = new Herb();
        } else if (type.equals("tanker"))
        {
            cookie = new darckChocolate();
        }
    }
}
```

``` java
public class KingDom {
    SimpleCookieFactory simpleCookieFactory; // 킹덤에는 간단한 CookieFactory가 존재한다.
    
    public KingDom(SimpleCookieFactory simpleCookieFactory) {
        this.simpleCookieFactory = simpleCookieFactory;
    }
    
    public Cookie drawCookie(String type) {
        Cookie cookie;
        
        cookie = simpleCookieFactory.createCookie(type);
        
        // 변하지 않는 부분
        cookie.prepare();
        cookie.bake();
        cookie.cut();

        return cookie;        
    }
    
    
}
```

쿠키런 킹덤이 지금처럼 아주 잘되서 글로벌 진출을 하는데 쿠키의 특성이 나라마다 맞게 나오게 되니다면? 

간단한 팩토리를 사용한 경우에는 모든 곳에서 동일한 팩토리에서 만들긴 하는데 약간 서로 다르게 만들기 시작한다면..?

미국 쿠키는 달아지고.. 일본 쿠키는 짜진다면..?!!





**많이 도움을 받고 있는 곳**

 https://jusungpark.tistory.com/14

https://biggwang.github.io/2019/06/28/Design%20Patterns/[Design%20Patterns]%20%ED%8C%A9%ED%86%A0%EB%A6%AC%20%ED%8C%A8%ED%84%B4,%20%EB%8F%84%EB%8C%80%EC%B2%B4%20%EC%99%9C%20%EC%93%B0%EB%8A%94%EA%B1%B0%EC%95%BC-%EA%B8%B0%EB%B3%B8%20%EC%9D%B4%EB%A1%A0%ED%8E%B8/



