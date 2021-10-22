---
title:  "cannot find symbol 'reactive'"
header:
  teaser: 
categories: 
  - 
tags:
  - 
---

vue에서 reactive 관련 공부를 시작하던 중 reactive를 찾을 수 없다는 오류가 발생.

강제를 import를 시켜도 vue에서 reactive를 찾을 수 없다는 오류가 계속해서 발생.

```javascript
import { reactive } from 'vue';
```

->

```ruby
"export 'createApp' was not found in 'vue'
```

결론부터 말하자면 뷰 버전의 문제였다.

vue 3 버전부터 reactive가 지원이 된다.

package.json에서 확인 가능하며 "vue --version" 명령어는 vue cli 버전을 말하는 것이었다.

그래서 vue 4버전인줄 알고 있었으나 2버전이었고

vue 3버전으로 설치하니 정상 작동 확인이 되었다.

```ruby
npm install --save vue@3
```
