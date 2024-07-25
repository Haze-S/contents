---
date: '2024-04-19'
title: 'CSS-in-JS'
categories: ['Note']
tags: ['React', 'WIL']
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/de38ff3f-aba9-4210-b4e4-67ddd2855a71'
---

![cssisawe](https://github.com/Haze-S/blog-contents/assets/87344625/de38ff3f-aba9-4210-b4e4-67ddd2855a71)

CSS-in-JS는 JavaScript 안에서 CSS를 작성하고 관리하는 방법을 말한다. CSS 코드를 별도의 파일이나 style 태그 안에서 작성하는 대신 JavaScript 객체 또는 함수를 사용하여 스타일을 정의하고 적용한다. 주로 React와 함께 사용되며, 대표적으로 Styled-components, Emotion 등의 라이브러리가 있다.

## 사용 방법

아래는 리액트에서 Styled-components를 사용한 예시이다.

```jsx
import React from 'react';
import styled from 'styled-components';

// 스타일이 적용된 div 요소를 생성
const StyledDiv = styled.div`
  color: ${(props) => props.color || 'black'};
  font-size: ${(props) => props.fontSize || '16px'};
  padding: 20px;
  background-color: ${(props) => props.bgColor || 'white'};
  border: 2px solid ${(props) => props.borderColor || 'transparent'};
  border-radius: 5px;
`;

const StyledButton = styled.button`
  background-color: ${(props) => (props.primary ? 'blue' : 'white')};
  color: ${(props) => (props.primary ? 'white' : 'blue')};
  font-size: 1rem;
  padding: 10px 20px;
  border: 2px solid blue;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: ${(props) => (props.primary ? 'darkblue' : 'lightblue')};
  }
`;

const StyledComponentsExample = () => {
  return (
    <div>
      {/* StyledDiv 컴포넌트에 스타일을 적용 */}
      <StyledDiv
        color="red"
        fontSize="20px"
        bgColor="lightgrey"
        borderColor="red"
      >
        This is a styled div.
      </StyledDiv>

      {/* StyledButton 컴포넌트에 스타일을 적용. */}
      <StyledButton primary>Primary Button</StyledButton>
      <StyledButton>Secondary Button</StyledButton>
    </div>
  );
};

export default StyledComponentsExample;
```

**`styled.div`** 와 **`styled.button`** 을 사용하여 각각 **`StyledDiv`** 와 **`StyledButton`** 컴포넌트를 생성하고, 해당 컴포넌트에 스타일을 적용했다. 스타일은 백틱( \` ) 안에 작성되며 `${}`를 사용하여 JavaScript 표현식을 포함할 수 있다. 만든 컴포넌트들은 JSX 코드에서 일반적인 React 컴포넌트처럼 사용할 수 있다.

### 장점

- 컴포넌트 기반 스타일링

  - CSS-in-JS는 컴포넌트 기반 접근 방식을 따르므로, 각 컴포넌트에 대한 스타일을 쉽게 정의하고 유지보수할 수 있다.

- 스타일 스코프 제한

  - CSS-in-JS는 스타일이 컴포넌트 범위 내에서만 적용되도록 보장하여 스타일 충돌을 방지한다.

- 동적 스타일링

  - JavaScript를 사용하기 때문에 동적으로 스타일을 변경하거나 조건부로 스타일을 적용하는 것이 용이하다.

- 선언적 스타일링

  - CSS-in-JS는 스타일을 선언적으로 작성하여 가독성을 높이고, 스타일의 목적을 더 명확하게 표현할 수 있다.

- 번들 최적화
  - 사용되지 않는 스타일을 제거하고 최적화된 CSS를 생성하여 번들 크기를 줄일 수 있다.

### 단점

- 서버 사이드 렌더링 복잡성

  - 서버 사이드 렌더링을 사용하는 경우에는 CSS-in-JS의 사용이 복잡할 수 있으며, 초기 로딩 성능에 영향을 줄 수 있다.

- 의존성

  - JavaScript에 의존하기 때문에 JavaScript가 비활성화되어 있는 경우에는 스타일이 적용되지 않을 수 있다.

- 빌드 과정 복잡성

  - CSS-in-JS를 사용하면 추가적인 빌드 단계가 필요할 수 있으며, 빌드 시간이 늘어날 수 있다.

- 스타일 시트 관리
  - 큰 규모의 프로젝트에서는 스타일을 관리하기가 어려울 수 있으며, 일관성 있는 스타일을 유지하기 위한 추가적인 노력이 필요할 수 있다.
  - CSS-in-JS는 프로젝트의 특성과 요구 사항에 따라 적절히 선택되어야 하며, 장단점을 고려하여 사용해야 한다.

## 웹을 스타일링하는 다양한 방법들

CSS-in-JS 이외에도 웹 애플리케이션에서 스타일링을 하는 다양한 방법이 있다.

- CSS 파일 분리

  - 가장 전통적인 방법으로, CSS 파일을 별도로 작성하여 HTML 문서에서 **`<link>`** 태그를 사용하여 연결하는 방식. 각 파일은 각각의 역할에 맞게 관리되며, 여러 페이지에서 재사용될 수 있다.

- CSS 프레임워크 및 라이브러리

  - Bootstrap, Bulma, Tailwind CSS 등과 같은 CSS 프레임워크나 라이브러리를 사용하여 UI를 디자인하고 스타일을 적용할 수 있다. 이러한 라이브러리는 사전에 디자인된 구성 요소와 유틸리티 클래스를 제공하여 개발 시간을 단축하고 일관된 디자인을 유지할 수 있다.

- CSS 전처리기

  - Sass(SCSS), Less, Stylus 등과 같은 CSS 전처리기를 사용하여 변수, 중첩, 믹스인 등의 기능을 활용하여 보다 유연하고 효율적으로 CSS를 작성할 수 있다. 전처리기를 사용하면 반복되는 코드를 줄이고 유지보수성을 높일 수 있다.

- CSS 모듈
  - CSS 모듈은 CSS 파일을 모듈로 분리하여 컴포넌트 스코프 내에서 스타일을 관리하는 방법이다. 각 컴포넌트에 대한 고유한 클래스명을 생성하여 스타일 충돌을 방지하고, 컴포넌트 단위로 스타일을 관리할 수 있다.

어떤 방법을 사용할 지에 대한 정답은 없다. 각각의 방법은 프로젝트의 특성, 규모, 팀의 선호도 등을 고려하여 선택되어야 한다.
