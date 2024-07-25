---
date: '2024-06-02'
title: 'Emotion JS'
categories: ['Note']
tags: ['Blog', 'CSS']
thumbnail: 'https://github.com/Haze-S/contents/assets/87344625/ec64e43d-9889-479b-a5de-cc3c8d02d9e9'
---

Emotion.js는 JavaScript 기반의 CSS-in-JS 라이브러리로, 스타일링을 위한 도구이다. Framework Agnostic, React 애플리케이션에서 사용되며 CSS와 JavaScript를 결합하여 컴포넌트 기반의 스타일링을 가능하게 한다.

기존에 많이 사용하던 라이브러리는 styled-components 라이브러리였다. 템플릿 리터럴 또는 객체를 통해 손쉽게 스타일을 적용한 컴포넌트를 만들 수 있었고, 손쉽게 적용할 수 있었기 때문이다. 또한 Sass 문법을 지원했기 때문에 더 간결한 코드 작성이 가능하고, 서버사이드렌더링을 지원해주기 때문에 이를 위해 추가적인 조치를 취할 필요가 없었다. 하지만 번들 용량이 CSS-in-JS 라이브러리 중 제일 컸는데, 이는 콘텐츠 제공에 큰 영향을 끼치기 때문에 아주 중요한 문제였다.

그러다 EmotionJS 라이브러리가 등장했는데 해당 라이브러리는 styled-components의 기능을 거의 동일하게 사용할 수 있었고 추가적인 라이브러리를 설치하여 기능을 손쉽게 확장할 수 있었다. 이런 기능을 그대로 구현한 라이브러리의 번들 용량이 다른 라이브러리에 비해 압도적으로 작다는 것이 EmotionJS가 인기를 얻을 수 있는 이유였다.

## EmotionJS 주요 특징

- CSS-in-JS
  - 스타일을 JavaScript 안에 직접 작성할 수 있다. 이를 통해 스타일과 컴포넌트 로직을 한 곳에서 관리할 수 있다.
- 다양한 스타일링 방식 지원
  - styled API, css 함수, 템플릿 리터럴, 객체 스타일링 등 다양한 스타일링 방법을 제공한다.
- 동적 스타일링
  - props를 기반으로 조건부 스타일링이 가능하며, 테마 기능을 활용하여 어플리케이션 전반에 걸쳐 일관된 스타일을 적용할 수 있다.
- 퍼포먼스 최적화
  - 런타임과 컴파일 타임 두 가지 모드를 제공하여 성능을 최적화할 수 있다. 컴파일 타임 모드를 사용하면 스타일을 미리 컴파일하여 런타임 오버헤드를 줄일 수 있다.
- 작은 번들 크기
  - Emotion은 작은 크기의 라이브러리로, 필요에 따라 다양한 기능을 선택적으로 사용할 수 있어 더 가볍게 유지할 수 있다.
- 서버사이드 렌더링(SSR) 지원
  - Emotion은 서버사이드 렌더링을 쉽게 설정할 수 있어, 초기 페이지 로딩 시간을 단축하고 SEO 성능을 개선할 수 있다.
- 높은 유연성과 범용성
  - Emotion은 React에만 국한되지 않고 다른 JavaScript 프레임워크와도 함께 사용할 수 있다.
  - 다른 CSS 프레임워크나 기존 CSS 파일과 함께 사용할 수 있어, 기존 프로젝트에 통합하기 용이하다.

## Styled-Component와의 차이점

- 유연성 및 범용성
  - Emotion.js: 다양한 스타일링 방법을 지원한다. styled API뿐만 아니라 css 함수, keyframes 등을 제공하여 더욱 유연한 스타일링을 가능하게 한다. Emotion은 React 외의 다른 프레임워크와도 사용할 수 있다.
  - styled-components: 주로 styled API를 중심으로 작동하며, React에 특화되어 있다.
- 성능
  - Emotion.js: 컴파일 타임과 런타임 두 가지 모드를 제공하여 성능 최적화가 가능하다. 컴파일 타임 모드를 사용하면 스타일을 미리 컴파일하여 런타임 오버헤드를 줄일 수 있다.
  - styled-components: 런타임 스타일링에 초점을 맞추고 있으며, 성능 최적화를 위해 다양한 내부 최적화가 되어 있지만, 컴파일 타임 최적화 기능은 없다.
- 번들 크기
  - Emotion.js: Emotion은 작은 크기의 라이브러리로, 필요에 따라 다양한 기능을 선택적으로 사용할 수 있어 더 가볍게 유지할 수 있다.
  - styled-components: 모든 기능이 하나의 패키지에 포함되어 있어 번들 크기가 더 클 수 있다.
- 사용자 경험
  - Emotion.js: 다양한 스타일링 옵션을 제공하여 유연성이 높지만, 설정이 다소 복잡할 수 있다. 이를 통해 더욱 세밀한 제어가 가능하지만, 학습 곡선이 있을 수 있다.
  - styled-components: 설정이 비교적 간단하며, 직관적인 API를 제공하여 학습하기 쉽다. React 개발자에게 친숙한 경험을 제공한다.
- 생태계 및 커뮤니티
  - Emotion.js: 점점 인기를 얻고 있는 라이브러리로, 커뮤니티와 생태계가 성장하고 있다.
  - styled-components: 오랜 기간 동안 많은 사용자를 확보한 성숙한 라이브러리로, 커뮤니티와 생태계가 매우 활성화되어 있다.

### 사용 예시

#### EmotionJS

```jsx
import { css } from '@emotion/react';

const style = css`
  color: hotpink;
`;

function App() {
  return <div css={style}>Hello, Emotion!</div>;
}
```

#### Styled-Component

```jsx
import styled from 'styled-components';

const StyledDiv = styled.div`
  color: hotpink;
`;

function App() {
  return <StyledDiv>Hello, styled-components!</StyledDiv>;
}
```

Emotion.js와 styled-components는 모두 훌륭한 CSS-in-JS 솔루션을 제공하지만, 프로젝트의 요구사항과 개발자의 선호도에 따라 선택할 수 있다. Emotion.js는 유연성과 성능 최적화에 강점이 있고, styled-components는 사용의 용이성과 React에 특화된 경험을 제공한다.
