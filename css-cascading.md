---
date: '2024-03-19'
title: '[CSS] Cascading'
categories: ['Note']
tags: ['Web', 'CSS', 'WIL']
summary: 'Cascading은 사전적 의미로 작은 폭포라는 뜻을 가지고 있다.'
thumbnail: '![cascading](https://github.com/Haze-S/blog-contents/assets/87344625/983d1d32-ea10-4c57-b2e7-853a2102b899)'
---

![cascading](https://github.com/Haze-S/blog-contents/assets/87344625/983d1d32-ea10-4c57-b2e7-853a2102b899)
CSS 의 중요한 핵심 개념 중 하나인 Cascading은 사전적 의미로 작은 폭포라는 뜻을 가지고 있다. 웹 페이지를 꾸미는 일에… 갑자기 왠 폭포? 이 폭포가 의미하는 바가 무엇인 지 알아봤다.

## Cascading Style Sheets

**CSS의 풀 네임은 Cascading Style Sheets**. 이름에 조차 포함된 이 단어는 CSS의 기본 동작에 대한 원리가 된다. 우리는 웹 사이트 만드는 법을 배우다 보면, 원래 사용하고 있던 웹 브라우저에도 기본적인 스타일이 적용되어 있다는 것을 알게 된다. 하지만 우리는 **같은 브라우저로 어떤 사이트를 가더라도 화면이 똑같이 생긴 사이트는 찾을 수 없다**. 이게 바로 **캐스케이딩의 원리**이다.

![Chrome 의 h2 태그에 적용된 기본 스타일](https://github.com/Haze-S/blog-contents/assets/87344625/1721b0e7-8a7c-4a44-b9a6-3eb6a1da0dbb)
_Chrome 의 h2 태그에 적용된 기본 스타일_

CSS는 스타일이 적용되는 우선순위가 있다. 폭포라는 의미에 빗대어 흔히들 계단식, 위에서 아래로 흐르는 순서로 되어 있다고 말하는데, 이는 **CSS 규칙이 순서에 따라 합쳐서 적용된다는 걸 의미한다.** 순서는 어떻게 정해지는 걸까? CSS는 아래 두가지 원칙을 통해 어디에 어떤 스타일을 적용할 지 결정된다.

## 1. 스타일 우선순위

CSS 사용에 있어서, 말 그대로 어떤 스타일을 먼저 적용할 것인지는 우선순위에 따라 결정한다. 우선순위는 중요도와 명시도 그리고 코드 작성 순서를 보고 결정하게 된다.

### a. 중요도

먼저 해당 사이트의 스타일이 적용된 위치에 따라서 중요도는 달라진다. 스타일 시트 위치는 크게 세 가지가 있는데, 사용자 에이전트, 작성자(개발자), 사용자로 나뉜다. 이 세 가지를 중요도에 따라 나열하면 아래와 같다.

> 작성자(개발자) > 사용자 > 사용자 에이전트

- **작성자(개발자) 스타일 시트**

가장 일반적인 유형의 스타일 시트로 웹 개발자가 작성한 스타일이다. 이는 사용자 에이전트 스타일을 새로 정의하거나 특정 웹 사이트의 디자인을 정의할 수 있다. 아래 3가지 방법 중 하나가 쓰인다.

1. html 파일에 `<link rel="stylesheet" type="text/css" href="style.css">` 링크
2. `<style>` 태그 블록 안에 작성
3. `<p style="color: red;"> 인라인 <p/>` 태그 안에 인라인 속성으로 지정

- **사용자 스타일 시트**

웹 사이트 사용자는 브라우저 환경을 원하는 대로 맞출 수 있도록 사용자 정의 스타일 시트를 설계하여 스타일을 재정의 할 수 있다. 사용자 에이전트에 따라 [직접 스타일을 구성](https://www.thoughtco.com/user-style-sheet-3469931)하거나 브라우저 확장을 통해 추가할 수 있다.

- **사용자 에이전트 스타일 시트**

브라우저에는 모든 문서에 제공되는 기본 스타일 시트가 있다. 위에서 캡쳐와 함께 언급한 user agent stylesheet 가 바로 이 것이다. 이는 속성 옆에 `!important` 중요한 속성이 포함되지 않는 한, 선택기의 특수성에 관계없이 작성자 스타일에 선언된 모든 스타일보다 우선순위가 낮다.

### **!important**

정해진 우선순위를 무시하고 의도적으로 중요도를 올릴 방법이 있다. 바로 스타일 속성에 `!important` 키워드를 사용하는 것. 이는 단순히 중요도를 높이고 싶은 속성 뒤에 붙이기만 하면 된다.

```css
셀렉터 {
  속성: 속성값 !important;
}
```

해당 키워드를 사용하면 우선순위가 제일 아래에 있더라도 먼저 사용될 수 있다. 단, 작성자와 에이전트 스타일에 모두 적용되어 있다면 기본적인 순서를 따른다. 아래는 우선순위에 따른 표이다.

![출처: mdn web docs](https://github.com/Haze-S/blog-contents/assets/87344625/8e3e8446-8c8b-4d90-8971-ce3cfbc4c858)
[_출처: mdn web docs_](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

폭포가 흐르듯이 계단식으로, 작성자 스타일 시트에 정의된 것이 없다면 사용자, 사용자에도 없다면 사용자 에이전트 스타일 시트의 스타일을 따른다.

### b. 명시도

우선순위를 정하는 두 번째 방법인 명시도는 셀렉터가 가리키는 것이 정확할수록 더 우선순위를 높게 둔다는 의미이다. 명시도를 높이는 것은 정확히 어떤 태그에 적용할 것 인가를 정하는 일이다.

```html
<!-- 1등 -->
<p id="id-selector" class="class-selector" style="color: red;">셀렉터</p>
```

```css
/** 2등 **/
#id-selector {
  color: blue;
}

/** 3등 **/
.class-selector {
  color: yellow;
}

/** 4등 **/
p {
  color: green;
}
```

하나의 p 태그에 인라인 스타일 속성과 id, class 가 정의되어 있고 각각의 속성에 스타일이 적용되어 있다. 여기서 **제일 먼저 우선되는 스타일은 인라인 스타일 속성**이다. 인라인 스타일은 태그 안에 정의되어 있는 속성이기 때문에 그 누구보다도 명확하게 어떤 태그에 스타일을 적용할 것 인지 알 수 있기 때문이다. 그 뒤로는 문서 안에서 딱 한 번만 등장되는 id가 두 번째 우선순위를 갖는다. 명시도가 높은 순으로 나열된 우선순위는 다음과 같다.

1. 태그 안에 _**직접적으로 정의되는 인라인**_ 스타일
2. 문서 내에 _**단 한번만 정의하는 id**_ 셀렉터
3. 문서 내에 _**여러번 사용이 가능한 class**_ 셀렉터
4. 문서 내 _**모든 태그를 가리키는 태그 셀렉터**_

### c. 코드 작성 순서

우선 순위를 정하는 마지막 방법은 코드를 작성한 순서이다. 일반적으로 코드는 위에서 아래로 읽어가며 프로그램을 실행하는데, CSS 또한 위에서 아래로 읽으며 실행한다고 생각하면 편하다. 아래와 같이 같은 태그에 같은 셀렉터와 속성을 사용해도 아래에 쓰여진 스타일이 적용된다.

<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/fcogvrku-the-solid/embed/OJGbQrd?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/fcogvrku-the-solid/pen/OJGbQrd">
  Untitled</a> by 송현정 (<a href="https://codepen.io/fcogvrku-the-solid">@fcogvrku-the-solid</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 2. 스타일 상속

캐스케이딩의 두 번째 원칙은 스타일 상속이다. 스타일 상속은 태그의 구조가 어떻게 생겼는지에 따라 적용되는 스타일을 결정하는 원칙이다. 아래 예시는 부모 태그와 자식 태그의 상속을 보여 준다.

내가 지정한 스타일이 아니더라도 부모 태그에 의해 상속되는 스타일이 있으니 CSS 를 적용할 때 유의해야 한다.

```html
<article style="color: red;">
  <p>나의 기본 색상은 Black입니다.</p>
</article>
```

![개발자 도구를 통해 어떤 태그에서 스타일이 상속 받았는지 알 수 있다.](https://github.com/Haze-S/blog-contents/assets/87344625/fd6a126e-619c-41bc-a4dc-7a6207923634)

개발자 도구를 통해 어떤 태그에서 스타일이 상속 받았는지 알 수 있다.

이렇게 CSS 의 Cascading 원칙을 알아봤다. CSS 의 이름에도 적혀있는 Cascading, 이는 CSS 를 지칭하는 근본이 되는 단어가 아닐까? 스타일을 적용할 때는 이 작은 폭포를 주의하며 사용을 해야겠다.
