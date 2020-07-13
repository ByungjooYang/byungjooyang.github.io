---
title:  "Srping 기초"
header:
  teaser: ""
categories: 
  - Spring
tags:
  - Spring
---

Spring은 자바 객체를 담고 있는 경량 컨테이너이다.

경량 컨테이너라는 것은 특정 기능 위주로 간단한 jar 파을 등을 이용해서 

모든 개발이 가능하도록 구성된 프레임워크란 뜻이다.

**스프링의 특성**

1. 객체의 생성, 소멸 등 라이프 사이클을 관리하며 스프링으로부터 객체를 얻어 올 수 있다.

2. 또한 POJO(Plain Old Java Object) 기반의 개발을 한다. 이는 어떤 상속도 안받고 implements도 안한다는 것이다.

3. 의존성 주입(DI)을 지원한다. 설정 파일을 통해서 객체간 의존 관계를 설정 할 수 있다.

4. AOP(관점 지향 프로그래밍)를 지원한다. OOP를 보완한다.

5. 트랜잭션 처리를 위한 방법을 제공한다,

6. 영속성과 관련되어 다양한 서비스를 제공한다.
  (myBatis, hibernate 등)

7. 동적인 웹 사이트를 개발하기 위한 여러 서비를 제공.

8. MVC Framework를 제공한다.

**★ DI (Dependency Injection)**
 : 스프링의 핵심 개념으로 객체간 의존관계를 자신이 아닌 외부에서 설정된다는 개념이다.

 설정 파일을 사용해 손쉽게(?) 객체 간 의존관게를 설정해 스프링을 DI 커테이너라고 부르기도 한다.

 DI컨테이너는 어떤 클래스가 필요로 하는 인스턴스를 자동으로 생성, 취득해 연결시켜준다.

DI 컨테이너가 인스턴스를 생성하도록 하려면 프로그램 소스 내부에서 new 로

직접 생성하지 않고 설정파일에서 필요로 하는 클래스의 정보를 설정해 줘야한다.

Constructor Injection과 Setter Injection 두 가지가 있다.

1. Constructor Injection
 : 생성자를 통해 의존관계를 연결시키는 것.
  생성자를 통해 의존관계를 연결하기 위해선 xml 설정 파일에 `<bean>`요소의 하위요소로 `<constructor-arg>`를 추가해 주어야 한다.
   객체를 전달할 경우에는 ref 요소를 써야한다.


2. Setter Injection
 : setter 메소드를 이용해 의존 관계를 연결시키는 것이다.
  property 요소의 name 속성을 이용해 값의 의존 관계를 연결시킬 대상이 되는 필드값을 지정한다.



**Spring Bean**  
 - 일반적으로 XML 파일에 정의한다.
 - 속성
  ~ class(필수) : 정규화된 자바 클래스 명
  ~ id : bean의 고유 식별자. 따라서 중복되지 않아야함.
  ~ scope : 객체의 범위
  ~ constructor-arg : 생성 시 생성자의 매개변수
  ~ property : 생성 시 bean setter에 전달할 인수
  ~ init method 와 destroy method

Bean Scope
1. singleton
 : 하나의 bean 정의에 대해 Spring loC Container내에 단 하나의 객체만 존재한다. default 값이다.

2. prototype
 : 하나의 bean 정의에 대해 다수의 객체가 존재할 수 있다.

3. request
 : 하나의 bean 정의에 대해 하나의 HTTP request 생명주기 안에 단 하나의 개체만 존재한다. 즉 각 HTTP request는 자신만의 객체를 갖는다.
  Web-aware Spring ApplicationContext 안에서만 유효.

4. session
 : 하나의 bean 정의에 대해 하나의 HTTP session의 생명 주기 안에 단 하나의 객체만 존재한다.
  Web-aware Spring ApplicationContext 안에서만 유효.

5. global session
 : 하나의 bean 정의에 대해 하나의 global HTTP session의 생명 주기 안에 단 하나의 객체만 존재한다. 일반적으로 portlet context안에서만 유효하다.
  Web-aware Spring ApplicationContext 안에서만 유효.