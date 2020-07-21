---
title:  "Spring Mybatis"
header:
  teaser: 
categories: 
  - Spring
  - Mybatis
tags:
  - Spring
  - Mybatis
---

**스프링과 JDBC**
 스프링은 JDBC를 비롯해 ORM 프레임워크를 지원한다.
(Mybatis, hibernate, JPA(Java Persistence API)
스프링의 목표는 인터페이스에 의한 개발인데 DAO는 데이터베이스에서 
데이터를 읽거나 쓰는 수단을 제공하기 위해 존재하며, 
반드시 인터페이스를 통해 외부에 제공돼야 한다.
서비스 객체는 인터페이스를 통해서 DAO에 접근한다 
서비스 객체를 특정 데이터 액세스 구현체에 결합시키지 않음으로써 테스트를 용이하게 한다
DAO인터페이스는 DAO구현과 서비스 객체 사이에서 느슨한 결합이 유지될 수 있게 한다

<img src="/assets/img/20200720/DAO.png">

스프링은 데이터베이스 연동을 위한 탬플릿 클래스를 제공함으로써 
Connection, Statement(PreparedStatement), ResultSet 등을 생성하고
처리한 다음 close(반환)하는 JDBC의 중복된 코드를 줄일 수 있다.