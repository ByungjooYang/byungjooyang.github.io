---
title:  "Spring Security 기본 설정"
header:
  teaser: 
categories: 
  - Spring Security
tags:
  - Spring Security
---

Spring MVC에서 시큐리티를 적용하는 기본 설정에 대해 araboza.

우선 pom.xml에 세 개의 dependency를 추가해 준다

version은 스프링과 같이 5.2.0으로 맞추었다.

```ruby
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-core</artifactId>
  <version>5.2.0.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-web</artifactId>
  <version>5.2.0.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-config</artifactId>
  <version>5.2.0.RELEASE</version>
</dependency>

```

그 다음 root-context가 들어있는 /WEB-INF/spring 에

Spring Bean Configuration File을 만들고 이름은

security-context 라고 하면 된다.

만들 때 혹은 만들고 나서 namespaces에 security를 추가로 선택해주면 된다.

그러면 여기서 네임스페이스는 beans와 security 두 개가 선택되게 된다.

그리고 그 안에 경로를 지정하고 권한을 부여하게 된다.

```ruby
<security:http>
  <security:intercept-url pattern="/all/*" access="permitAll"/>
  <security:intercept-url pattern="/member/*" access="hasRole('ROLE_MEMBER')"/> 
  <!-- hasRole 값은 ROLE_* 형태로 작성해야 한다. -->
	<security:intercept-url pattern="/admin/*" access="hasRole('ROLE_ADMIN')"/>

	<security:form-login login-page="/all/loginForm" />
</security:http>

```

여기서 pattern은 권한을 부여할 경로를 지칭하고 

access는 권한을 지정한다.

member 폴더 안의 뷰의 권한은 MEMBER 권한을 지닌 사용자만이

접근이 가능하다는 뜻이며

security:form-login 은 로그인 창 폼을 말하고 login-page를

지정하지 않을시 스프링에서 기본 제공하는 로그인 창으로 가게 된다.

그 다음으로는 web.xml에 필터를 만들어 줘야 한다.

```ruby
<filter>
  <filter-name>springSecurityFilterChain</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>

<filter-mapping>
  <filter-name>springSecurityFilterChain</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

```

그리고 context-param에 root-context.xml 밑에 security-context.xml이 있음을

알려주어야 한다.

/WEB-INF/spring/security-context.xml 을 추가해 주자.

```ruby
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
  /WEB-INF/spring/root-context.xml
  /WEB-INF/spring/security-context.xml
  </param-value>
</context-param>

```

**security-context.xml 관련 추가 수정사항**

<img src="/assets/img/20200803/5.2.png">

나 같은 경우 처음에spring-security-5.1.xsd 로 되어있었다.

그래서 security:http 태그를 쓸때부터 에러가 떴었는데 

내가 dependency에 추가해 줬던 5.2 버전으로 수정해 주었더니

에러가 사라졌다.


**web.xml**

<img src="/assets/img/20200803/web.png">

이렇게 설정하고 나면 member폴더 안에 있는 뷰를 보기 위해

url을 입력해도 로그인을 하지 않았으니 loginForm으로 이동하게 된다.

**intercept-url의 access 값들 (SqEL 문법)**

1. permitAll : 모든 접근 허용
2. denyAll   : 모든 접근 불허
3. hasRole('role') 
    : 지정한 role 권한이 있어야 접근이 가능. 지정된 role을 가지고 있으면 true를 반환한다.
4. hasAnyRole('role1, role2') 
    : role1, role2 권한 모두 접근 가능. 둘 중 하나라도 가지고 있으면 true를 반환한다.
5. isAnonymous() 
    : 인증을 사용하지 않은 사용자일 경우 (로그인을 하지 않은 사용자일 경우) true를 반환
6. isRememberMe() 
    : remember-me 기능으로 로그인 한 사용자일 경우 true를 반환
7. isAuthenticated() 
    : 인증을 사용한 사용자일 경우 true 반환
8. isFullyAuthentiated() 
    : anonymous사용자가 아니고 remember-me 기능으로 로그인 하지 않은 사용자일 경우 true를 반환

9. principal 
    : 현재 사용자를 나타내는 principal 객체에 직접 접근할 수 있음
10. authentication 
    : SecurityContext로 부터 얻은 Authentication 객체에 직접 접근할 수 있습니다.

11. hasPermission(Object target, Object permission) 
    : 사용자가 주어진 권한으로 제공된 대상에 액세스 할 수 있으면 true 를 반환
12. hasPermission(Object targetId, String targetType, Object permission)