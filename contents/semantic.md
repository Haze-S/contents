---
date: "2024-03-19"
title: "[HTML] 시맨틱 태그(Semantic tag)"
categories: ["Web", "HTML", "WIL"]
summary: "시맨틱 태그란 이름과 같이 의미있는, 뜻을 가지고 있는 태그를 뜻한다."
thumbnail: "https://github.com/Haze-S/blog-contents/assets/87344625/e9c05255-e28c-453e-9933-03cc98c1e541"
---

![semantic1](https://github.com/Haze-S/blog-contents/assets/87344625/e9c05255-e28c-453e-9933-03cc98c1e541)

시맨틱 태그란 이름과 같이 의미있는, 뜻을 가지고 있는 태그를 뜻한다.
그렇다면 의미가 없는 태그, 존재 가치가 없는 태그가 있단 말인가? 시맨틱 태그가 뜻하는 바를 생각해본다.

## Who are you?

우리가 흔히 쓰는 **웹 페이지는 하나의 문서**라고 배워왔다. 문서가 있고, 문서 내에는 구역 내지 **박스라는 공간이 존재**한다. 웹 페이지가 아닌 다른 문서를 예로 들면, 박스들은 논문의 서론, 본론, 결론 혹은 신문 기사의 제목, 부제, 리드 문장 등이 될 것 같다.

문서인 웹 페이지는 보통 head, main, footer 로 나뉜다. 페이지의 제목, 다른 페이지의 링크 등이 들어있는 head, 페이지의 본격적인 내용인 main, 사이트의 부가적인 정보 및 링크가 있는 footer. 모든 박스들은 각자의 정해진 일이 있다. 이 박스들을 하나의 태그로 퉁 쳐버리면 어떻게 될까? 아래 예시로 우리의 만능 태그 `<div>` 를 사용해서 퉁 친 문서를 작성해봤다.

<details>
<summary><b>&ltdiv&gt 지옥 맛보기</b></summary>
<div markdown="1">

```html
<body>
  <div id="header">
    <div class="nav"></div>
    <div class="figure">
      <div class="figcaption"></div>
    </div>
  </div>
  <div id="main">
    <div class="section">
      <div class="article">
        <div class="time"></div>
        <div class="mark"></div>
        <div class="details">
          <div class="p_tag">
            <div class="summary"></div>
          </div>
        </div>
      </div>
      <div class="figure">
        <div class="figcaption"></div>
      </div>
    </div>
    <div class="section">
      <div class="article">
        <div class="time"></div>
        <div class="mark"></div>
        <div class="details">
          <div class="p_tag">
            <div class="summary"></div>
          </div>
        </div>
      </div>
      <div class="figure">
        <div class="figcaption"></div>
      </div>
    </div>
  </div>
  <div id="aside">
    <div class="article">
      <div class="details">
        <div class="summary"></div>
      </div>
      <div class="figcaption"></div>
    </div>
  </div>
  <div id="footer">
    <div class="nav"></div>
    <div class="figure">
      <div class="figcaption"></div>
    </div>
  </div>
</body>
```

</div>
</details>

<p align="center">
  <img src="https://github.com/Haze-S/blog-contents/assets/87344625/e93de4a1-e9a4-400e-9dea-1eaaf1204555" width="300" />
</p>

진짜 온통 `<div>`, 중간에 `<p>` 조차 `<div>` 로 작성했다. 이렇게 태그를 의미없이 써버리면 웹 사이트를 이용하면서 챙길 수 있는 다양한 이점들을 놓치게 된다. 또한 개발자들의 가독성을 떨어트려 업무 생산력이 저하될 수도 있다. 그렇다면 이 `<div>` 들에게 어떤 이름을 주면 좋을까?

## 시맨틱 태그 종류

<p align="center">
  <img src="https://github.com/Haze-S/blog-contents/assets/87344625/56a112f2-58d2-4d25-a868-b6012821240d" width="300"/>
</p>

시맨틱 태그에는 여러 종류의 태그들이 있다. 당연히 문서의 구조에 맞게 태그에 의미를 부여해야하니 그런 것도 있고, 다양한 기능 및 페이지 안내를 위해 보다 자세히 정해진 것 같다.

### 주로 사용되는 시맨틱 태그

<details>
<summary><code>&ltheader&gt</code></summary>
<div markdown="1">

- 문서의 머릿글을 담당한다.
- 웹 페이지에서 로고나 광고 배너, 로그인 링크 등을 삽입한다.

</div>
</details>

<details>
<summary><code>&ltnav&gt</code></summary>
<div markdown="1">

- navigation 의 약자로 웹 사이트를 ‘항해’할 때 사용된다.
- 웹 사이트에서 각종 페이지의 링크들을 넣어 다른 페이지로 이동할 때 사용된다.

</div>
</details>

<details>
<summary><code>&ltmain&gt</code></summary>
<div markdown="1">

- 문서의 본론, 해당 페이지의 주요 내용을 담당한다.
- 주요 내용을 한 곳에 모은 큰 틀이라 생각하면 되겠다.

</div>
</details>

<details>
<summary><code>&ltsection&gt</code></summary>
<div markdown="1">

- 본론 안에서 한 구간을 담당하는 태그이다.
- 논문에도 본론 안에 목차가 있듯이 하나의 목차를 담당하는 구간이다.

</div>
</details>

<details>
<summary><code>&ltarticle&gt</code></summary>
<div markdown="1">

- 해당 태그안의 내용은 그 내용만으로 하나의 문서가 완성된다.
- 따라서 본문 안에 넣어 사용해도 되고 따로 `<article>`만 사용해도 된다.

</div>
</details>

<details>
<summary><code>&ltaside&gt</code></summary>
<div markdown="1">

- 본문과 직접적인 내용은 아니지만 연관된 부가적인 내용을 쓸 때 사용된다.
- 보통 웹 사이트에선 사이드 메뉴 바로 이용이 되곤 한다.

</div>
</details>

<details>
<summary><code>&ltfooter&gt</code></summary>
<div markdown="1">

- 문서의 바닥글을 담당한다.
- 웹 사이트의 약관이나 채용 링크 등 본문의 내용과 관련은 없지만 이용에 필요한 내용을 넣는다.

</div>
</details>

### 추가적인 시맨틱 태그

<details>
<summary><code>&ltdetails&gt</code></summary>
<div markdown="1">

![semantic4](https://github.com/Haze-S/blog-contents/assets/87344625/9e510239-1310-49a9-90bb-ed37ea83f211)

- 해당 태그는 ‘열림’상태일 때 정보가 표기되는 태그이다.
- `<summary>` 태그에 정보의 이름(제목)을 넣고 사용한다.

</div>
</details>

<details>
<summary><code>&ltsummary&gt</code></summary>
<div markdown="1">

- `<details>` 태그 안에 작성되어 해당 정보의 이름을 담당한다.

</div>
</details>

<details>
<summary><code>&ltfigure&gt</code></summary>
<div markdown="1">

![semantic5](https://github.com/Haze-S/blog-contents/assets/87344625/94ff9c6c-fa0e-43b3-af88-c607115a2327)

- 하나의 독립적인 컨텐츠(이미지, 영상 등)를 표현한다.
- `<figcaption>` 태그를 이용해 해당 컨텐츠에 설명을 쓸 수 있다.

</div>
</details>

<details>
<summary><code>&ltfigcaption&gt</code></summary>
<div markdown="1">

- `<figure>` 태그 안에 작성되어 해당 요소의 설명을 담당한다.
- 이미지의 캡션과 같은 일을 한다.

</div>
</details>

<details>
<summary><code>&ltmark&gt</code></summary>
<div markdown="1">

- 내용 안에서 중요한 부분에 하이라이트를 표시한다.
- 책에 형광펜으로 줄을 긋는 것과 유사하다.

</div>
</details>

<details>
<summary><code>&lttime&gt</code></summary>
<div markdown="1">

- 시간의 특정 지점 혹은 구간을 나타낼 때 사용한다.
- 보다 적절한 검색 결과나 알림 같은 특정 기능을 구현할 때 사용된다.

</div>
</details>

## 시맨틱 태그의 장점

시맨틱 태그를 사용할 때 갖게 되는 이점은 단순히 태그의 의미를 생각하며 작성하는 것 보다 더 좋은 효과들이 있다. 단순히 이름을 짓고 구간을 나누는 것이 아닌, 그에 따르는 장점들은 아래와 같다.

### 웹 접근성 증가

웹 접근성이란 장애인이나 고령자분들이 사이트에서 제공되는 정보를 비장애인과 동등하게 접근하고 이용할 수 있도록 보장하는 것이다. 한국에는 [웹접근성인증평가원](https://www.wa.or.kr/index.asp)이 있을 정도로 중요한 요소이다. 이는 한국 뿐만아니라 인터넷을 사용하는 어느 나라에서든 이를 지키려고 노력하며, 웹의 창시자인 팀 버너스리도 중요히 생각한다.

> **_“ 웹의 힘은 보편성에 있습니다.
> 장애에 상관없이 모두가 접근할 수 있다는 것이 가장 중요한 부분입니다.”_**
> -- 팀 버너스리, W3C 디렉터 및 World Wide Web의 창시자

시맨틱 태그를 사용하게 되면 해당 컨텐츠가 문서의 본론인 지 아니면 부가적인 정보인 지 알 수 있게 된다. 이는 장애를 가진 사용자들이 원활한 서비스 이용을 위해 사용하는 도구(스크린 리더기 등)에게도 정확한 정보를 전달 할 수 있게 된다. 도구에게 정확한 정보를 줌으로서 사용자도 정확한 정보를 얻게 할 수 있다.

### SEO (Search Engine Optimization)

SEO는 말 그대로 검색 엔진 최적화를 뜻한다. 우리는 흔히 웹 서핑을 할 때, 구글이나 네이버 등 검색 엔진 사이트를 통해서 필요한 정보를 얻는다. 정보를 검색하다 보면 상위에 노출되는 사이트가 있는 반면 하단에 노출되어 사용자들이 스크롤을 내려야만 보이는 사이트가 있다. 이런 기준은 어떻게 정해지는 지 생각해보면 SEO의 중요성을 알 수 있다. 각 사이트마다 검색 작동 방식과 가이드는 다르겠지만 그 기본이 되는 것이 시맨틱 태그이다.

시맨틱 태그를 사용하면 검색 엔진이 사용자가 원하는 정보를 보다 정확하게 찾을 수 있고 그 정보가 정확할 수록 상단에 노출시킬 가능성이 크다. 더불어 운영하는 서비스가 상단에 노출된다면 이는 많은 트래픽을 유도하고 최종적으로 서비스의 매출에 많은 기여를 하게 된다.

SEO를 높이는 방법에는 메타 태그 활용, alt 속성 기재 등 많은 방법들이 있지만 시작은 문법에 맞는 html 작성이 진행되어야 한다.

### 업무 생산성 저하

우리는 글 초반에 `<div>` 로만 이루어진 html 문서를 예로 보았다. 개발자의 입장에서 실제 서비스가 저렇게 이루어져 있다면 어떨까. html 문서에서 조차 찾기 힘들었던 article 부분은 html 에 연결되어 사용되는 CSS, JS 파일에 까지 영향을 끼칠 것이다. 예를 들어 `<div class="links">` 라고만 표시되어 있고 해당 태그를 CSS에서 셀렉터로 사용한다고 치자. 이 태그는 어떤 링크들을 모은 태그인 지, 해당 링크들은 어떤 동작을 하는 지 한눈에 알아보기 힘들다. 이런 문제는 서비스 개발을 바로 시작해야 하는 상황에서 개발 전 태그의 의미를 찾는 행동을 함으로서 업무 시간에 불필요한 행동으로 시간을 낭비하게 된다.

우리의 소중한 시간을 세이브하기 위해서는 시맨틱 태그를 적극적으로 활용하여 가독성을 높일 필요가 있다.

## div는 가치가 없다?

시맨틱 태그의 뜻은 의미있는 태그라고들 말한다. 시맨틱 태그에 예시를 들 때마다 따라오는 태그가 있는데, 이는 `<div>` 태그와 `<span>` 태그이다. 보통 `<div>` 로 이루어진 문서들을 나쁜 예로 들고는 한다. 그렇다면 `<div>` 태그의 사용은 운영하는 서비스에 악영향을 미칠까?

나는 그렇게 생각되지 않는다. 트럼프 게임에서 joker는 어떤 카드의 대신으로 쓸 수 있는 카드이다. 누구보다 막강하고 어떤 상황에서든 이길 수 있는 조건을 만들어 준다. `<div>` 는 html 문서에서 조커가 아닐까 생각한다. `<div>`는 역전할 수 있는 그리고 무엇이든 대체 가능한 만능, 절대로 없어서는 안되는 태그이다.
