---
layout: post
title: 매개변수(parameter)와 인수(argument)의 차이
category: misc
created_date: 2022-02-03
modified_date: 2022-02-03
author: duchangkim
tags: [word]
search_keyword: [매개변수, 인수, parameter, argument]
---
***

# 매개변수(Parameter)
매개변수는 변수의 특별한 한 종류로서, 함수에 투입되는 **변수**를 의미합니다.

```js
function add(num1, num2) {
  ...
}
```

위 `add`함수에서 `num1`, `num2`가 바로 매개변수 입니다.

# 인수(Argument) 또는 전달인자
함수를 호출할 때 전달하는 **값**을 의미합니다.

```js
add(1, 3);
```
위 `add` 함수의 사용부에서 `1, 3`이 바로 인수(전달인자) 입니다.

***

이렇게 매번 헷갈리는 용어를 정리 해봤는데요. 이제는 진짜 안까먹고 잘 기억 해야겠습니다.  
매개변수는 `변수`라는 점에 주목하고, 전달인자는 함수 사용부에서 `전달한다` 라는 것에 주목하면 다시는 잊어버리지 않을 것 같습니다.
