---
date: '2024-03-28'
title: '[JS] var & let & const'
categories: ['Note']
tags: ['JavaScript', 'WIL']
summary: '이런 var보같은 녀석!'
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/eed5aa00-f42b-43c8-a62b-0952fb1d4b39'
---

![js-variable1](https://github.com/Haze-S/blog-contents/assets/87344625/eed5aa00-f42b-43c8-a62b-0952fb1d4b39)

과거의 JavaScript는 변수를 지정할 때 `var` 키워드와 함께 사용했다. `var` 는 위의 밈처럼 ~~마치 고양이같이 제 멋대로인 구석이 있다.~~ 처음 JS를 만들 때 왜 그렇게 만들었을까 배경이 궁금해서 찾아봤지만 너무 오래된 역사였는지 잘 찾아지지가 않았다. 변수 하나에 데이터 타입, 스코프 등 챙겨야 하는 것이 많아서 그 스트레스를 덜고자 이런 컨셉으로 만든걸까…? 느슨한 컨셉덕분에 `var` 는 오히려 개발자들에게 긴장감을 주었고 이를 대처할 방안으로 새로운 키워드인 `let` 과 `const` 가 생겨나게 됐다.

## 중복 선언

변수는 선언과 동시에 초기화(값 할당)가 가능하며 선언만 하는 경우로 여럿 있다.

#### `var` : 가능

`var`의 제일 큰 특징이자 문제점 중 하나는 중복 선언이 가능하다는 점이다. 이미 값이 할당되어 있는 변수에 또 다른 값을 선언하거나 혹은 선언만 하고 싶었던 변수에 나도 모르는 값이 들어있을 수도 있다.

```jsx
.
.
11 var test = '이거?';
12 console.log(test);  // 이거?
.
.
.
282 var test;
283 console.log(test);  // 이거?
.
.
.
484 var test = '그거 아니야';
485 console.log(test);  // 그거 아니야
```

위의 예제와 같이 개발 초기에 `test` 라는 변수에 값을 할당했다고 가정하자. 수 많은 코드들이 짜여진 후 282번 줄에서 `test` 의 존재를 잊은 채 필요에 의해 `test` 변수를 선언만 하려고 했다. 하지만 나도 모르는 새에 `test` 엔 원하지 않던 값이 할당되어 있었다. ~~두둥~~

혹은 나와 같이 일하는 동료 개발자가 `test` 의 존재를 모르고 484번 줄에서 같은 이름의 변수를 중복 선언을 한 뒤 작업을 이어 한다면 내가 해당 파일을 읽을 때 `test` 는 11번 줄의 `test` 이고 동료 개발자의 `test` 는 484번이 될 것이다. 같은 파일을 두고 서로 다른 작업을 하게 될 수도 있는데 이 두 작업은 하나의 변수로 인해 많은 에러와 예상하지 못한 액션을 유발할 수 있다.

#### `let` , `const` : 불가능

위의 문제점을 보완하기 위해 `let` 과 `const` 는 중복 선언이 불가능하게 만들었고 만약 중복 선언이 된다면 에러가 발생하여 즉시 실행을 멈추게 된다. 따라서 변수의 값을 변경해야 할 경우 중복 선언이 아닌 재할당을 통해 값을 변경할 수 있다.

```jsx
let letTest = '이거?';
console.log(letTest); // 이거?
letTest = '그거 아닐걸';
console.log(letTest); // 그거 아닐걸
let letTest;
console.log(letTest); // syntaxError
```

하지만 **`const` 는 상수를 표현할 수 있는 키워드로서 중복선언은 물론이고 재할당 또한 불가능**하다.

```jsx
const constTest = '하이';
console.log(constTest); // 하이
constTest = '안녕 못 해';
console.log(constTest); // TypeError
```

다만 `const` 키워드로 선언된 변수가 객체 또는 배열 (reference 타입)을 가리키게되는경우, 해당 변수에 선언된 값은 객체 또는 배열의 참조 값 (reference 값) 이므로 객체 또는 배열의 내부 속성과 요소 모두 변경이 가능하다.

## 스코프

변수는 보통 전역 변수와 지역 변수로 나뉘게 되는데 전역 변수는 해당 문서의 어디에서나 사용 가능한 변수이고 지역 변수는 정해진 스코프 안에서만 사용이 가능한 변수이다. 즉, 스코프는 유효 범위를 나타낸다.

#### `var` : 함수 스코프

`var` 의 특이점은 **_지역 변수의 스코프가 함수로 한정_** 되어 있다는 것이다. 이 말은 즉 함수를 제외한 for, if 등과 같은 **_구문 안에서 선언된 변수가 그 구문 밖에서도 유효_** 했다.

```jsx
function test() {
  var varTest = '함수 안';
  console.log(`이 곳은 ${varTest} 입니다`);
}

test(); // 이 곳은 함수 안 입니다
console.log(varTest); // ReferenceError: varTest is not defined

for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i); // 5
```

이런 컨셉은 중복 선언과 비슷한 이유로 많은 문제를 일으켰다.

#### `let` , `const` : 블록 스코프

위와 같은 이슈를 해결하기 위해 새로 만들어진 `let` 과 `const` 의 유효 범위는 `{}` 블록으로 구분하게 되었다. 함수와 여러 구문을 뿐만 아니라 단순히 중괄호로 감싸진 코드 블록으로도 유효 범위가 구분된다.

```jsx
function test() {
  const varTest = '함수 안';
  console.log(`이 곳은 ${varTest} 입니다`);
}

test();
console.log(varTest); // ReferenceError

for (let i = 0; i < 5; i++) {
  console.log(i);
}

{
  let language = 'JavaScript';
}

console.log(i); // ReferenceError
console.log(language); // ReferenceError
```

## 호이스팅

호이스팅(hoisting)이란 자바스크립트에서 선언 전에 호출(사용)된 변수나 함수 등이 사용 전에 미리 선언한 것 같이 보이게 하는 것이다. 선언된 코드 자체가 문서 상단으로 올라가진 않지만 실행 컨텍스트에 의해 그렇게 보이게 한다.

#### `var` : undefined

`var` 의 경우 호이스팅을 완전 확실하게 보여줄 수 있는 예시이다. 사용 후에 변수를 선언하게 되더라도 사용 전에 미리 선언이 되어있던 것 같은 현상이 나타난다.

```jsx
console.log(a); // undefined

var a = 5;
console.log(a); // 5
```

이런 현상은 코드의 흐름을 읽는 데 방해가 되기도 하며, 의도치 않은 값으로 인해 에러가 발생하기도 한다.

#### `let` , `const` : referenceError

`var` 와 같이 초기화 되지 않는다. 따라서 선언 전에 사용을 할 경우 에러가 발생하여 코드 실행이 중지 된다.

```jsx
console.log(a); // ReferenceError

let a = 5;
console.log(a); // 5
```

이는 `let` 과 `const` 이 호이스팅이 발생하지 않는 것처럼 보이지만, 사실은 변수의 선언과 초기화 사이에 일시적으로 변수 값을 참조할 수 없는 구간인 TDZ(Temporal Dead Zone)에 빠졌기 때문에 보이는 현상이다.

자바스크립트 내부에서는 코드 실행 전 변수 선언만 할 뿐 초기화는 코드 실행 과정에서 선언문을 만났을 때 수행한다. 그렇기 때문에 `let` 과 `const` 도 호이스팅이 발생하기는 하지만, 값을 참조할 수 없기 때문에 동작하지 않는 것처럼 보이는 것이다.
