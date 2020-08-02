---
title:  "Spring MVC에서 AJAX 사용 시 success로 안넘어가는 문제"
header:
  teaser: 
categories: 
  - Spring
tags:
  - Spring
---

불과 엊그제와 어제 spring mvc 프로젝트를 하던 도중 문제를 발견했다.

ajax를 사용하여 DB를 거쳐 로그인과 회원가입을 하는데

DB에 정상적으로 가지만 ajax에서 success가 작동하지 않는

괴랄한 문제가 생긴것이다.

controller에서 DTO를 가져온것도 Sysout으로 찍어도 잘 나오고

회원가입 할 때도 DB에 잘 들어가지만 

success가 작동을 안하는 것이다.

그런데 확률형으로 success가 값을 비워두고

버튼을 클릭했을 때 작동을 하기도 했다.

아무튼 이래저래 검색을 하루 죙일 해보고 

어떤짓을 해봐도 안되었다.

일단 success 가 안됐을 때를 검색해보면 

Controller에 @ResponseBody 를 안붙여 줬을 때라고 한다.

근데 내 코드엔 이쁘게 잘 있었는데도 안되었다.

게다가 post 방식으로 했는데도 불구하고 get 방식 처럼 자꾸 

주소에 id와 pw 가 그대로 붙어버리는 것이다. 

이는 회원가입도 마찬가지 였고 (여기서 눈치 챘어야 했는데)

하루 종일 고민한 결과 

`<input type="button">`이 아니라 `<button>` 태그를 사용했었는데

button 태그는 타입을 지정해주지 않으면 default가 submit 이라서 였다.

따라서 type="button" 을 부여하니 잘 작동되었다.

```ruby
<button type="button" id="loginBtn">LOGIN</button>
<button type="button" onclick="location='javascript:history.go(-1)'">BACK</button>
```

`<form action=''>` 이 상태였기 때문에

method가 디폴드 값인 get이라 주소창에 값이 그대로

보였던 것이었다.

그런데 action이 빈 문자열인데도 불구하고 제이쿼리가 

먹혔다는것이 이상할 따름이다.

아마 id값이 같으니 제이쿼리가 먹히다 만것 같다.



이것을 발견하고 현타가 씨게 와버렸다.. 

나의 무지의 소치이니 어쩌겠나.. 이렇게 배워가는 것이라 생각해야겠다.

  