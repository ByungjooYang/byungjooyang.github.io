---
title:  "StompClient, Sock.js 사용시 invalidStateError 문제 해결"
header:
  teaser: 
categories: 
  - 
tags:
  -
---

필자는 스프링에서 채팅방을 구현하던 도중 Stompclient와 sock.js 를 알게되었다.

이는 websocket에서 채팅방을 나누기 어려웠던것을 쉽게 만들어주는 역할을 하는 라이브러리인데

채팅방이 하나일 때는 괜찮으나 여러개 일 때 이 에러가 발생한다.

"InvalidStateError : The connection has not been established yet"

정말 많은 검색을 해보았고 중국 사이트에 들어가서 수 많은 한자들 사이에서 코드를 찾으며

고생을 했지만 해결책은 찾을 수 없었다.

아 해결책을 있었지만 angular와 php 등 스프링에서의

해결책은 아니었다.

물론 따라해 보았지만 전혀 해결되지 않았었다.

며칠을 고민하다 결국 sock.js stmopclient.js 파일 내부를 뒤져보기 시작했다.

에러 이름에서도 유추할 수 있듯이 이 에러는

상태가 유효하지 않을때 발생하는 에러인데

stompclient는 내부적으로 subscribe 되고나서

웹소켓의 send 메서들 호출한다.

<img src="/assets/img/20200927/connecting.png">

그런데 이 메서드를 호출할 때 상태가 CONNECTED 일때 보내지고

CONNECTING 상태일 때 위의 에러를 throw 시키는 것이다.

근데 채팅방이 여러개 일 때 하나를 subscribe 시키고 

ws.send 시킬 때 다음 채팅방을 subscribe 시키게 되면

위의 에러를 발생시키게 되는 것 같다(뇌피셜..)

그래서 그런지 새로고침을 여러번 하면 아다리가 맞을때는 

위의 에러가 뜨지 않을 때도 있었다.

그래서 라이브러리의 js 코드를 조금만 손 볼까 하다가

시간이 급해 일단 subscribe 시킬 때 setTimeout을 사용해

시간을 지연시켜 subscribe를 시켰더니 에러가 안뜨고

잘 작동이 되었다.

<img src="/assets/img/20200927/subscirbe.png">

물론 좋은 해결책은 아니지만 우선 시간이 급하다면

이 방법을 쓰는것도 나쁘지 않은 것 같다.