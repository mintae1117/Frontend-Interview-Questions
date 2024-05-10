# ES6에서 Arrow 함수를 언제, 왜 쓸까?

<br>

Arrow 함수(arrow function)를 언제, 왜 사용하는지 그 이유들을 알아봅시다.

<br>

## 1. 함수 본연의 기능을 아주 잘 표현하는 문법입니다.

보통 프로그래밍할 때 function 문법은 아래와 같은 이유로 많이 사용합니다.

- 여러가지 기능을 하는 코드를 한 단어로 묶고 싶을 때 (그리고 나중에 재사용할 때)
- [입출력기능](#gear-입출력기능)을 만들 때

<br>

그리고 `arrow function`을 사용하면 함수 본연의 입출력기능을 아주 직관적으로 잘 표현해줍니다.

<br>

```js
let 두배만들기 = (x) => {
  return x * 2;
};

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br>

`arrow function`을 쓰면 입출력기능이 쉽고 예쁘게 표현되는 것이 `arrow function`를 쓰는 이유 중 하나입니다.

<br>

<br>

## 2. 소괄호 생략이 가능합니다.

파라미터가 하나라면 소괄호를 생략 가능합니다.

<br>

```js
let 두배만들기 = x => {
  return x * 2;
};

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br>

이렇게도 가능합니다.

<br>

<br>

## 3. 중괄호 생략이 가능합니다.

중괄호 안에 return 한줄 뿐이라면 중괄호와 return도 생략가능합니다.

<br>

```js
let 두배만들기 = (x) => x * 2;

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br>

생략하니 이제 x가 어떻게 변하는 함수인지 입출력기능이 바로 한눈에 들어오는 걸 볼 수 있습니다.

<br>

<br>

## 4. `arrow function`을 쓰면 내부에서 `this`값을 쓸 때 밖에 있던 `this`값을 그대로 사용합니다.

`arrow function`은 어디서 쓰던 내부의 `this` 값을 변화시키지 않습니다. <br>

또한 바깥에 있던 `this`의 의미를 그대로 내부에서도 사용합니다. 이게 `arrow function`을 쓰는 핵심 이유입니다. <br>

예시를 보겠습니다.

<br>

```js
let 오브젝트1 = {
  함수: function () {
    console.log(this);
  },
};

오브젝트1.함수();
```

<br>

위의 코드는 실행하면 무슨 결과가 나올까요? <br>

> 결과: 함수()를 가지고 있는 오브젝트인 오브젝트1이 콘솔창에 출력됩니다.

<br>

다른 예시를 봅시다.

<br>

```js
let 오브젝트1 = {
  함수: () => {
    console.log(this);
  },
};

오브젝트1.함수();
```

<br>

위의 코드는 출력하면 어떤 결과가 나올까요? <br>

> 결과: `window`라는게 출력됩니다. 여기선 `this`가 `window`입니다. <br>
> 함수의 주인인 오브젝트1이 출력되지 않는 이유는 `this`값은 함수를 만나면 항상 변하는데, <br> > `arrow function` 안에서는 변하지 않고 밖에 있던 `this`를 그대로 씁니다. <br>
> (오브젝트 밖에 있던 `this`는 `window`입니다.)

<br>

왜냐면 `arrow function`은 외부에 있던 `this`를 그대로 내부로 가져와서 사용하는 함수기 때문입니다. <br>

내가 예측하던 `this`값과 달라질 수도 있으니 이는 장점이 아니라 단점이 될 수도 있습니다.

`this`의 뜻이 달라지는 것 처럼 일반 `function`과 용도가 완전 같지 않기 때문에

일반 `function`을 항상 대체할 수 있는 문법이 아닙니다. 그것만 주의합시다.

---

<br>

## :hammer_and_wrench: 용어 공부

### :gear: 입출력기능

<br>

2를 집어넣으면 x + 2를 출력해주는 함수를 어떻게 만들어쓸까요?

<br>

```js
function 더해주세요(x) {
  return x + 2;
}
```

<br>

위와 같은 문법을 이용해서 만들어 사용합니다. 함수의 소괄호안에는 `input` 역할을 하는 `파라미터`가 있고 <br>

함수내에 `return` 이라는 것은 `output` 역할을 하는 키워드입니다. 그럼 이제 더해주세요(2); 이렇게 사용하면? <br>

4가 이 자리에 남게 됩니다. 소괄호에 뭔가 집어넣으면 return을 이용해 뭔가 뱉어내는 것. 이게 바로 함수의 입출력 기능입니다.

<br>

## 참고

- [코딩애플, Arrow function은 function을 대체하는 신문법이 아님](https://codingapple.com/unit/es6-3-arrow-function-why/)
