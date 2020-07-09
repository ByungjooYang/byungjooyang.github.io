---
title:  "2020-07-09 스프링 수업 시작 및 스프링 설정"
header:
  teaser: ""
categories: 
  - Spring
tags:
  - Spring
---

처음에 스프링 설정하는 방법들은 여러가지가 있고 검색만 하면 많이 나오니 여기서 다 설명하지는 않겠지만 특별히 기억할 만한 것을 포스팅 해보겠다.

이클립스에서 스프링을 설정할 때 Spring legacy Project를 만들고 jar 파일을 여러 개 끌고 와야한다.

<img src="/assets/img/20200709/jarfile.png">

이렇게 해도 되고

아니면 스프링 프로젝트를 만들고 프로젝트를 우클릭 -> configure -> Convert to Maven Project을 해줘도 된다.

![img](/assets/img/20200709/convert.png)

이렇게 해서 만들 땐 porm.xml이 만들어 지는데 그 안에 이렇게 설정을 해줘야 한다.
dependencies 태그를 를 만들고 http://www.mvnrepository.com 에 들어가서 spring context 검색 후 맨 위의 파일에 들어가서 Maven 부분을 복사해서 방금 만든 그 태그 안에 복사 붙여넣기를 해야한다.

<img src="/assets/img/20200709/Maven.png">

<img src="/assets/img/20200709/prom.png">

그러고 나서 스프링 환경설정 xml 파일을 만들어 줘야하는데 일반 xml 파일로 만드는 것이 아니라 

Spring Bean Configuration File로 만들어야 한다.


<img src="/assets/img/20200709/context.png">

그리고 만들 때 바로 finish를 누르는게 아니라 next를 눌러 내가 쓸 요소들을 (여기서는 beans와 context를) 선택해줘야한다.

만약 바로 finish를 눌러도 밑에 namespace를 누르면 선택할 수 있으니 걱정하지 말자.

<img src="/assets/img/20200709/namespace.png">

