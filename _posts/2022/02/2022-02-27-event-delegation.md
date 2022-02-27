---
layout: post
title: 이벤트 위임(Event delegation)
category: JS
created_date: 2022-02-27
modified_date: 2022-02-27
author: duchangkim
tags: [js]
search_keyword: [js, event]
---
***

# 이벤트 위임
이벤트 위임은 동적으로 노드를 생성하고 삭제할 때 유용하게 사용됩니다. 노드를 동적으로 생성하고 삭제할 때 각 노드에 대한 이벤트 리스너를 추가하지 않고, 상위(부모) 노드에서 하위 노드의 이벤트를 제어하는 방식입니다. 물론 동적으로 생성하고 삭제하지 않는 경우에도 사용할 수 있습니다.

## 사용

```jsx
// HTML
<ul>
	<li id="chicken">닭가슴살</li>
	<li id="protein">프로틴쉐이크</li>
	<li id="bar">프로틴바</li>
	<li id="beef">소고기</li>
</ul>

// JS
const ul = document.querySelector('ul');
const li = document.querySelector('li');

ul.addEventListener('click', (e) => {
	// li태그 외에는 이벤트 무시
	if (e.target.nodeName !== 'LI') {
		return;
	}
	if (e.target.id === 'chicken') {
		alert('닭가슴살 맛있다.')
	} else if (e.target.id === 'protein') {
		alert('딸기맛 프로틴 맛있따.')
	} else if (e.target.id === 'bar') {
		alert('프로틴바 벌레나올까봐 안먹는다.')
	} else {
		alert('개좋아');
	}
});
```

## 문제점
위의 예제처럼 간단한 (li 태그 안에 text node만 있는)경우에서는 크게 문제가 없습니다. 하지만 조금 복잡해지거나, svg를 활용한 아이콘 버튼을 사용하거나 할 때 문제가 발생합니다...

```jsx
// HTML
<ul>
	<li>
		<button id="chicken">
			<svg>
				<g></g>
			</svg>
		</button>
	</li>
	<li>
		<button id="beef">
			<svg>
				<g></g>
			</svg>
		</button>
	</li>
</ul>

// JS

ul.addEventListener('click', (e) => {
	// button태그 외에는 이벤트 무시
	if (e.target.nodeName !== 'BUTTON') {
		return;
	}
	if (e.target.id === 'chicken') {
		alert('닭가슴살 맛있다.')
	} else if (e.target.id === 'beef') {
		alert('소고기 개좋아.')
	} 
});
```

버튼을 누르면 잘 작동할 것처럼 생겼지만, 실제로 `console`에 `e.target`을 찍어보면, `<svg>, <g>, <button>`이 클릭하는 부분에 따라 다르게 나오는 것을 알 수 있습니다. 이러면 `button#beef`를 제대로 찾을 수 없습니다.

이 문제는 간단히 CSS로 해결할 수 있습니다.

```css
/* CSS */
button > * {
	pointer-events: none;
}
```
이렇게 해주면 버튼 어디를 눌러도 e.target은 button이 됩니다.

이벤트 위임 패턴 뿐만 아니라 비슷한 다른 문제를 해결할 수 있을 것 같습니다.

---

References

[https://css-tricks.com/slightly-careful-sub-elements-clickable-things/](https://css-tricks.com/slightly-careful-sub-elements-clickable-things/)