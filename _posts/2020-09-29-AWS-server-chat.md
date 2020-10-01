---
title:  "Sock.js 사용 시 localhost에서는 연결되나 AWS 서버에 올리면 연결이 안되는 문제"
header:
  teaser: 
categories: 
  - Sock.js
tags:
  - Sock.js
---

Sock.js를 이용해 웹채팅을 구현을 했는데

로컬호스트에서는 잘 작동이 되지만 AWS 서버에 올리고 난 후

404에러와 403에러가 엄청나게 뜨기 시작했다.

404 에러이니 결국에 도메인 문제라 생각을 했고

여러 군데 찾아 본 결과 메서드 하나만 추가해 주면 되었다.

나의 경우는 WebSocketMessageBrokerConfigurer를 구현해서

endpoint를 추가해주었는데

```ruby

@Override
public void registerStompEndpoints(StompEndpointRegistry registry){
  registry.addEndpoint("/chat").withSockJS();
}

```

여기서 endpoint가 /chat인데 도메인이 기본적으로

`http://localhost:8080/chat` 에서만 작동하게 되는듯했다.

그래서 확인해보니 setAllowedOrigins("*")를 추가해주면 끝이었다.

```ruby

@Override
public void registerStompEndpoints(StompEndpointRegitry registry){
  registry.addEndpoint("/chat").setAllowedOrigins("*").withSockJS();
}

```

앞에 와일드 카드가 붙은것으로 보아

앞에 어떤 것으로 시작하든 /chat으로 끝나는 경우

채팅과 연결시키는 것으로 보면 될듯 하다.