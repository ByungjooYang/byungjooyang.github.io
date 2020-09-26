---
title:  "JDBC DataSource 관련 수행속도 차이"
header:
  teaser: 
categories: 
  - 
tags:
  -
---

DataSource를 생성하려고 검색을 하다보면

BasicDataSource와 DriverManagerDataSource

보통 이 두가지를 사용하는 듯 하다.

그래서 이 둘의 차이가 무엇인지 알아봤다.

공식 문서를 보면 DriverManagerDataSource

```ruby
This class is not an actual connection pool; 
it does not actually pool Connections. 
It just serves as simple replacement for a full-blown connection pool,
implementing the same standard interface, 
but creating new Connections on every call.

Useful for test or standalone environments.
```
해석해 보자면 

'이 클래스는 실제 커넥션 풀이 아니다; 

실제 커넥션 풀 작업을 수행하지 않으며

단지 간단하게 full-down 커넥션 풀 작업을 대체하고, 

같은 표준 인터페이스를 구현한다. 그러나 매번 

부를때 마다 새 커넥션을 생성한다.

테스트 환경과 독립된 환경에서 유용하다.'

BasicDataSource는 J2EE 컨테이너 외부에 실제 연결 풀을 제공한다.

따라서 일반적으로 사용시 BasciDataSource를 사용하는 것이 좋다.

수행 속도면에서도 BasicDataSource가 빠르고 균일하다.

DriverManagerDataSource의 경우 수행속도가 속도가 느리고 균일하지도 않다.

관련해서 포스팅을 발견해 사진을 첨부한다.

확인

<img src="/assets/img/20200805/datasource2.png">

[출처][dataSource]


[dataSource]: https://ls-al.tumblr.com/post/40004576494/basicdatasource-vs-drivermanagerdatasource
