---
date: "2024-04-22"
title: "[JS] truthy & falsy"
categories: ["JavaScript", "WIL"]
summary: "알고 쓰면 유용한 아이들"
thumbnail: "https://github.com/Haze-S/blog-contents/assets/87344625/9710b7e5-013d-4432-88b8-4410b0e00e41"
---

<p align=center>
  <img src="https://github.com/Haze-S/blog-contents/assets/87344625/9710b7e5-013d-4432-88b8-4410b0e00e41">
</p>

JavaScript에서는 조건문이나 논리 연산자에서 다양한 값을 평가할 때 "falsy(거짓 같은)"와 "truthy(참 같은)" 값으로 분류된다. 이들은 조건식이나 논리 연산에서 어떻게 평가되는지를 결정한다.

### **Falsy 값**

- **false**: 명시적으로 false인 불리언 값
- **0**: 숫자 0
- **-0**: 음의 숫자 0
- **0n**: BigInt 0
- **''** 또는 **""**: 빈 문자열
- **NaN**: 숫자가 아님(Not-a-Number)
- **null**: null 타입의 값
- **undefined**: undefined 타입의 값

### **Truthy 값**

- **true**: 명시적으로 true인 불리언 값
- 모든 숫자(except 0)
- 모든 문자열(except '' 또는 "")
- 모든 객체, 배열, 함수
- **Infinity**: 양의 무한대
- **Symbol**: 심볼 값

## 서비스에서 응용되는 경우

아래의 예시는 서비스에서 falsy와 truthy를 사용하여 다양한 상황에 대응하는 방법이다. 이를 통해 조건부 로직을 효과적으로 처리하고 사용자 경험을 개선할 수 있다.

### 1. 사용자 인증 확인

`user`값이 `null`인 경우 falsy하기 때문에 로그인에 실패한다.

```jsx
if (user) {
  // 사용자가 로그인한 경우
  displayUserProfile();
} else {
  // 사용자가 로그인하지 않은 경우
  displayLoginButton();
}
```

### 2. 폼 유효성 검사

`inputValue` 가 `null` 인 경우 falsy하기 때문에 폼 전송을 하지 않고 에러 메세지를 출력한다.

```jsx
if (inputValue) {
  // 사용자가 유효한 값을 입력한 경우
  submitForm();
} else {
  // 사용자가 유효하지 않은 값을 입력한 경우
  displayErrorMessage();
}
```

### 3. 조건부 스타일링

```jsx
if (isError) {
  // 에러가 발생한 경우
  element.style.color = "red";
} else {
  // 에러가 발생하지 않은 경우
  element.style.color = "black";
}
```

### 4. 동적 요소 생성

```jsx
if (showButton) {
  // 버튼을 표시해야 하는 경우
  createButton();
}
```

### 5. 조건부 랜더링

```jsx
if (isAdmin) {
  // 관리자 계정인 경우
  renderAdminDashboard();
} else {
  // 일반 사용자인 경우
  renderUserDashboard();
}
```

## 코테나 알고리즘에서도 활용할 수 있다.

### 1. 문자열 검증

```jsx
function isValidString(str) {
  return str && typeof str === "string" && str.length > 0;
}
```

### 2. 배열 요소 유효성 검사

```jsx
function containsElement(arr, element) {
  return arr && Array.isArray(arr) && arr.includes(element);
}
```

### 3. 숫자 연산

```jsx
function multiplyIfTruthy(a, b) {
  return a && b && a * b;
}
```

### 4. 객체 속성 검증

```jsx
function hasRequiredProperties(obj) {
  return (
    obj &&
    typeof obj === "object" &&
    obj.hasOwnProperty("prop1") &&
    obj.hasOwnProperty("prop2")
  );
}
```

### 5. 조건부 로직

```jsx
function performActionIfTruthy(value) {
  if (value) {
    // value가 truthy한 경우에만 실행되는 로직
    console.log("Action performed!");
  }
}
```

개인적으로 Js의 truthy와 falsy는 아주 유용하고 편리한 기능이라고 생각한다. 활용도 어렵지 않고 그저 단순하게 해당 변수에 값이 제대로 들어있지 않으면 99% falsy한 경우라고 생각하면 된다.~~(암묵적 형변환 주의)~~ 유효성 검사나 조건부 로직을 짤 때 너무 유용하게 쓰고 있어서 JavaScript를 더 좋아하게 된다😆
