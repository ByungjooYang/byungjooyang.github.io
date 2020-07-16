---
title:  "spring MVC project 설정"
header:
  teaser: ""
categories: 
  - Spring
  - MVC
tags:
  - Spring
  - MVC
---

스프링 mvc 프로젝트를 하기위한 아주 초기의 설정을 포스팅 하겠다.

아주 쉬운 과정이지만 쉬울수록 조금만 안하면 더 잘 까먹기 때문이다.

우선 Dynamic Web Project로 프로젝트를 만든 후 
(바로 finish 하지말고 next를 눌러 web.xml를 만드는 것이 좋다.)

그리고 필요한 jar 파일을 가져오는데 필요한 목록은 22가지로 다음과 같다.

<img src="/assets/img/20200716/jar.png">

그리고 우클릭을 해서 add spring을 해준다.

<img src="/assets/img/20200716/nature.png">

만약 Maven을 이용할 경우

convert Maven을 한 후 porm.xml에 dependencies에 webmvc를 추가해 줘야한다.

버전 마다 다르겠지만 나는 5.2.0 버전을 썼다.

```ruby
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>5.2.0.RELEASE</version>
	</dependency>
```

이렇게 Dynamic Web Project를 이용한 방법도 있고 

Spring Legacy project로 만들 수 도 있다.

다음과 같이 Simple java가 아니라 Spring MVC Project를 선택하고

프로젝트 명을 입력한다.

<img src="/assets/img/20200716/legacy.png">

그리고 next를 눌러 패키지 명을 지정해야한다.

<img src="/assets/img/20200716/springMVC.png">

패키지는 세개 만들어야 하며 앞의 두 개는 아무거나 지정해도 되나

마지막 패키지 명은 프로젝트 명으로 하는 것이 더 좋다.

예전에는 마지막 패키지 명이 프로젝트 명과 다르면 에러가 났다고 하는데

지금은 에러가 나지 않는다.

하지만 그래도 웬만하면 프로젝트 명을 쓰는 것이 좋다.


**스프링 MVC 관계도**
<img src="/assets/img/20200716/sMVC.jpg">