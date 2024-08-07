---
date: '2024-03-25'
title: '얕은 복사 & 깊은 복사'
categories: ['Note']
tags: ['JavaScript', 'WIL']
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/41fc3205-43cb-489c-8d86-1e3ddc901f67'
---

![js-copy1](https://github.com/Haze-S/blog-contents/assets/87344625/41fc3205-43cb-489c-8d86-1e3ddc901f67)

내가 학교 다닐 때 배우고 기억하고 있는 깊은 복사와 얕은 복사는 위와 같은 그림이다.(값에 의한 전달, 참조에 의한 전달 등으로 부르기도 했다) 변수 A에 저장되어 있는 값을 필요에 의해 복사할 때, 얕은 복사는 변수 A의 값을 B에 그대로 복사하지 않는다. 대신 변수 A의 메모리 주소가 B에 저장된다. 이와 같은 매커니즘은 우리 컴퓨터의 메모리 공간은 한정적이고 추가적인 메모리가 필요할 땐 비용이 들기 때문이다. 때문에 변수들도 아나바다 운동에 참여해야 한다. ~~아껴야 잘 산다!~~

언어마다 메모리를 활용하기 위한 방법들이 있는데, JavaScript에서의 복사 방법을 정리해 보려고 한다.

## 원시 타입과 객체 타입

우선 복사를 알아보기 전에 JS에서 제공하는 데이터 타입을 알아볼 필요가 있다. 이렇게 타입을 나눈 건 복사와 연관이 있기 때문이다. 우선 이 두 가지의 타입은 세 가지의 측면에서 다른 점이 있다.

|           | 값 변경     | 변수에 할당  | 전달 방법    |
| --------- | ----------- | ------------ | ------------ |
| 원시 타입 | 변경 불가능 | 실제 값 저장 | 원시 값 복사 |
| 객체 타입 | 변경 가능   | 참조 값 저장 | 참조 값 복사 |

위와 같이 변수에 할당하거나 복사 값을 전달할 때 데이터 타입에 따라 어떤 값을 저장할 지 차이가 생긴다. 원시 타입과 객체 타입의 종류는 아래와 같다.

- 원시 타입
  - 숫자(number) : 정수와 실수 구분 없이 하나의 숫자 타입만 존재
  - 문자열(string) : 문자(char)와 문자열의 구분 없이 하나의 문자도 문자열로 인식
  - 불리언(boolean) : 논리적인 참(true)과 거짓(false)
  - undefined : 초기화 되지 않은 변수나 속성이 없는 객체 등에 할당되는 값
  - null : 의도적으로 값이 없음을 명시할 때 사용되는 값
  - 심볼(symbol) : 다른 값과 중복되지 않는 유일무이한 값
- 객체 타입
  - 객체, 함수, 배열 등

자바스크립트에서 원시 타입의 데이터들은 모두 값에 의한, 깊은 복사가 진행된다. 따라서 복사(전달 방법)에 의한 데이터의 주의는 객체 타입일 경우에만 해당된다.

## 참조에 의한 전달 : 얕은 복사(Shallow Copy)

![js-copy2](https://github.com/Haze-S/blog-contents/assets/87344625/f4a16147-eca4-4a8c-97f7-0725377e1ff9)

얕은 복사는 복사한 변수가 원본 변수에 할당된 값이 아닌 변수의 주소 값을 가지는 방법이다. 두 개의 변수가 동일한 메모리 공간에 저장된 데이터를 가리키고 있으므로, 둘 중 하나가 데이터를 변경하게 된다면 변경을 행동하지 않은 변수도 따라서 데이터가 변경이 된다.

```jsx
const user = {
	name : "lee",
	age : 99,
}
conset user2 = user;
user2.name = "kim";
//모두 똑같은 객체를 바라보고 있음
console.log(user); // {name:"kim", age:99}
console.log(user2); // {name:"kim", age:99}
```

`=` 와 같은 할당 연산자 외에 다른 방법으로도 복사가 가능하다.

### 얕은 복사 방법

- **Array.prototype.slice()**
  - start부터 end 인덱스까지 기존 배열에서 추출하여 새로운 배열을 리턴하는 메소드
  - start와 end를 설정하지 않는다면, 기존 배열을 전체 얕은 복사
- **Object.assign()**
  - 메소드의 첫 번째 인자로 빈 객체를 넣고 두 번째 인자로 복사할 객체를 넣는다.
- **Spread 연산자 (전개 연산자)**

## 값에 의한 전달 : 깊은 복사(Deep Copy)

![js-copy3](https://github.com/Haze-S/blog-contents/assets/87344625/878bb7aa-7da1-440c-bc33-b608c0121a99)

깊은 복사는 원본 변수에 있는 값을 복사하여 다른 메모리 공간에 적재한 뒤 해당 주소를 복사한 변수가 가리키는 것이다. 깊은 복사의 경우 서로 다른 데이터 주소를 가리키고 있기 때문에 어느 한 쪽의 값이 변경되어도 다른 한 쪽의 데이터에 영향을 주지 않는다.

```jsx
const a = 'a';
let b = 'b';
b = 'c';

console.log(a); // 'a'
console.log(b); // 'c'
// 기존 값에 영향을 끼치지 않는다.
```

### 깊은 복사 방법

- **JSON.parse && JSON.stringify**
  - JSON.stringify()는 객체를 json문자열로 변환하는데 이 과정에서 원본 객체와의 참조가 끊어진다.
  - 객체를 json 문자열로 변환 후,JSON.parse()를 이용해 다시 원래 자바스크립트 객체로 만든다.
  - 간단하고 쉽지만 비교적 느리다는 것과 객체가 function일 경우,  undefined로 처리한다는 것이 단점
- **재귀 함수를 구현한 복사**
- **Lodash 라이브러리 사용**

## 사실은…

위에서 `값에 의한 전달` , `참조에 의한 전달` 이라고 표현했지만 결국 식별자(변수)가 기억하는 메모리 공간에 저장된 값을 복사해서 전달한다는 면은 동일하다. JavaScript는 참조에 의한 전달은 존재하지 않고 값에 의한 전달만이 존재한다고 말할 수 있다. 다만 자바스크립트에선 포인터가 존재하지 않기 때문에 포인터가 존재하는 다른 언어의 전달이란 의미가 정확히 일치하지 않을 수 있다는 점을 기억해야한다.
