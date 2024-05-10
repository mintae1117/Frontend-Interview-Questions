# 이벤트 전파

생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM트리를 통해 전파됩니다.

<br>

> ### [이벤트 버블링](#gear-이벤트버블링)
>
> 이벤트가 하위 요소에서 상위 요소 방향으로 전파

<br>

> ### [이벤트 캡처링](#gear-이벤트캡처링)
>
> 이벤트가 상위 요소에서 하위 요소 방향으로 전파

<br>

+) 이벤트 버블링과 캡처링를 막기 위해서 [e.stopPropagation()](#gear-stoppropagation)을 사용합니다.
해당 웹 API를 통해 이벤트가 전파되는 것을 막을 수 있습니다.

<br>

> ### 타깃 단계
>
> 이벤트가 이벤트 타깃에 도달

<br>

### [이벤트 위임](#gear-이벤트위임): 이벤트 버블링 활용하기

이벤트 위임을 사용하지 않고, 동일한 이벤트를 일일히 수동으로 달아주기에는 코드 낭비가 너무 심합니다.

따라서 부모 요소에 이벤트를 부여해 버블링을 통해 하위 요소를 동작시킬때도 해당 이벤트가 발생하도록 만드는 것이 바람직합니다.

<br>

아래와 같은 상황에서

```html
<div class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">1</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item2">2</label>
  </li>
</div>
```

- case1: 각각 이벤트들을 부여해주기
  inputs의 내부 input에 각각 이벤트를 달아주었습니다.

```js
let inputs = document.querySelectorAll('input');
inputs.forEach((input) => {
  input.addEventListener('click', () => {
    alert('clicked');
  });
});
```

- case2: 부모 요소에 이벤트 부여하기
  부모 요소인 itemList를 클릭했을 때, 이벤트 버블링을 통해 checkbox type을 클릭했을 경우, 이벤트가 똑같이 동작하도록 만들었습니다.

```js
let itemList = document.querySelector('.itemList');
itemList.addEventListener('click', (e) => {
  console.log(e);
  if (e.target.type === 'checkbox') {
    alert('click');
  }
});
```

이처럼 이벤트 버블링을 통한 이벤트 위임은 하위 요소에 각각의 이벤트를 붙이지 않고도 상위 요소에서 하위 요소의 이벤트들을 제어할 수 있습니다.

<br>

---

<br>

## :hammer_and_wrench: 용어 공부

### :gear: 이벤트버블링

- 이벤트 버블링(Event Bubbling)은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미합니다. 아래와 같은 그림처럼요.

<img src="../../Images/important-4/event-bubbling.png" width="600px">

> 하위의 클릭 이벤트가 상위로 전달되어 가는 그림

### :gear: 이벤트캡처링

- 이벤트 캡처링(Event Capturing)은 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.

<img src="../../Images/important-4/event-capturing.png" width="600px">

> 클릭 이벤트가 발생한 지점을 찾아내려 가는 그림

<br>

```html
<div class="outside">
  녹색 영역
  <div class="middle">
    하늘색 영역
    <div class="inner">
      핑크색 영역
      <div class="float">회색</div>
    </div>
  </div>
</div>
```

<br>

```js
const outside = document.querySelector('.outside');
const middle = document.querySelector('.middle');
const inner = document.querySelector('.inner');
const float = document.querySelector('.float');

function callback() {
  alert(this.className + ' is Capturing!');
}

outside.addEventListener('click', callback, true);
middle.addEventListener('click', callback, true);
inner.addEventListener('click', callback, true);
float.addEventListener('click', callback, true);
```

<br>

위 코드는 이벤트 캡처링의 예시입니다. `float`을 클릭하면 `가장 상위 부모요소`인 `outside`의 이벤트부터 발생합니다. 이때 `addEventListener`함수의 `두번째 인자`로 전달된 `true`는 `이벤트를 캡처링해야하는지 여부`를 지정합니다.

<br>

```js
target.addEventListener(type, listener[, useCapture]);
```

<br>

- `type` : 이벤트의 이름을 지정하는 문자열. 대소문자 구별. (click, keypress 등..)
- `listener` : 이벤트가 발생할때 호출할 이벤트 리스너 함수.
- `useCapture` : 캡쳐링을 사용할지 여부를 지정하는 Boolean. default는 false 입니다. (선택사항)

### :gear: 이벤트위임

- 캡처링과 버블링을 활용하면 강력한 이벤트 핸들링 패턴인 이벤트 위임(event delegation) 을 구현할 수 있습니다.
  이벤트 위임은 비슷한 방식으로 여러 요소를 다뤄야 할 때 사용됩니다. 이벤트 위임을 사용하면 요소마다 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있습니다.
  공통 조상에 할당한 핸들러에서 `event.target`을 이용하면 실제 어디서 이벤트가 발생했는지 알 수 있습니다. 이를 이용해 이벤트를 핸들링합니다.

### :gear: stopPropagation

- “난 이렇게 복잡한 이벤트 전달 방식 알고 싶지 않고, 그냥 원하는 화면 요소의 이벤트만 신경 쓰고 싶어요.”라고 생각하시는 분들이 충분히 있을 수 있습니다. 실제로 마감 기한에 쫓기는 상황에서 이런 동작 방식을 정확히 이해하는 시간보다는 구현에 더 많은 시간을 쏟아야 하기 때문입니다. 그럴 때는 아래처럼 stopPropagation() 웹 API를 사용합니다.

```js
function logEvent(event) {
  event.stopPropagation();
}
```

위 API는 해당 이벤트가 전파되는 것을 막습니다. 따라서, 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해합니다. 그리고 이벤트 캡쳐의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않습니다.

<br>

## 참고

- [blog, 프론트엔드 면접 문제 은행](https://velog.io/@wkahd01/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A9%B4%EC%A0%91-%EB%AC%B8%EC%A0%9C-%EC%9D%80%ED%96%89-HTML-%EC%A7%88%EB%AC%B8-%EB%8B%B5%EB%B3%80#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%A0%84%ED%8C%8C)
- [blog, 이벤트 버블링,캡처,위임](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
- [blog, 이벤트 버블링(Bubbling) & 캡쳐링(Capturing)](https://thisblogfor.me/web/bubblingcapturing/)
