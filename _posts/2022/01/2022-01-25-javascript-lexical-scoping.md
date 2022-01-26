---
layout: post
title: JavaScript의 lexical scoping
category: JS
created_date: 2022-01-25
modified_date: 2022-01-25
author: duchangkim
tags: [js]
search_keyword: [렉시컬 스코핑]
---

***
# 서론

클로저(Closure)를 이해하기 위해서는 자바스크립트가 변수의 유효범위를 어떻게 지정하는지를 먼저 이해해야 합니다.

# Lexical scoping

```js
function init() {
	var name = 'kjsp';

	function displayName() {
		alert(name);
	}
}

init();
```

`init()`은 지역변수 `name`과 함수 `displayName()`을 생성합니다. `displayName()`은 `init()` 안에 정의된 함수이므로 `init()`함수 본문에서만 사용할 수 있습니다. `displayName()` 내부에는 자신이 가지고 있는 지역변수가 없습니다. 하지만 함수 내부에서는 외부 함수의 변수에 접근이 가능하기 때문에 부모 함수`init()`에서 선언된 변수 `name`을 접근할 수 있습니다.

위 코드도 Lexical scoping의 한 예입니다. 다른 예도 한번 살펴봅시다.

```js
var name = 'kjsp';

function displayName() {
	alert(name);
}

function init() {
	var name = 'duchi';
	displayName();
}

init();
```

위 코드에선 alert에 어떤 단어가 나올까요? `init()`함수에서 정의된 'duchi'가 alert에 나올까요? 정답은 'kjsp'가 출력됩니다.

`displayName()`의 name은 전역변수 name을 참조하고 있기때문입니다. 이것또한 lexical scoping이라고 합니다. [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures#%EC%96%B4%ED%9C%98%EC%A0%81_%EB%B2%94%EC%9C%84_%EC%A7%80%EC%A0%95lexical_scoping)에서 나와있듯이 한글로 변역하자면 **어휘적 범위 지정**인데, 한글로 번역하니 더 알 수 없는 말이 되어버렸습니다. **정적 스코프**([zerocho블로그](https://www.zerocho.com/category/JavaScript/post/5740531574288ebc5f2ba97e) 인용)라고 부르는 것도 좋아보입니다.

함수를 처음 선언하는 순간, 함수 내부의 변수는 자신의 스코프로부터 가장 가까운 곳(상위)에 있는 변수부터 찾아서 **계속 참조**하게 됩니다. 자신의 함수에서 찾아서 있다면 참조하고, 없다면 밖으로 나가서 찾아 참조하고 이런 방식입니다.

그래서 `displayName()`에서 참조하고 있는 `name`은 `init()`의 `var name = 'duchi'`이 아니라 `displayName()`과 가장 가까운 `var name = 'kjsp'` 를 참조하게 되는 것이죠.

![글쓰기는-5](https://user-images.githubusercontent.com/68454100/150979588-8ba27946-d3c9-43cc-a88c-20a1b7803ab3.jpg)


무슨 짓을 해도 displayName() 함수가 선언된 이상 displayName()함수가 참조하고 있는 전역변수 name을 다른 걸 참조하게 만들 수 없습니다.

# 요약

스코프는 함수를 호출할 때가 아니라 함수를 어디에 선언하였는지에 따라 결정된다. 이를 렉시컬 스코핑(Lexical scoping)라 한다.