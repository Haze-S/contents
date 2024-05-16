---
date: '2024-05-11'
title: '[Project] Styled Component 적응기'
categories: ['React', 'Project', 'Rolling']
summary: '장단점이 너무나도 명확한 그에 대하여..'
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/9cf35baa-f478-4d8a-9770-9f4348949f46'
---

이번 프로젝트에서 우리는 다양한 CSS 스타일 기법중에 Styled Component를 택했다. 이유는 이게 더 좋다기보단 안 써본 기술을 써보고 기술에 대해 공부할 겸 이에 대한 장단점을 느끼고 다음에 도구를 고를 때 참고하기 위해서다.

## Styled Component

![styled component](https://github.com/Haze-S/blog-contents/assets/87344625/9cf35baa-f478-4d8a-9770-9f4348949f46)

styled component에 대해 간략히 정리하자면 별도의 파일로 관리하던 CSS 코드를 컴포넌트 내에서 컴포넌트와 함께 모듈화되어 관리하는 기법이다. 서로 다른 모양새의 코드가 한 파일에 존재하여 기존의 방식으로 관심사를 분리하던 사람에겐 거부감이 들 수도 있다. 나 또한 그랬다.

styled component에서는 관심사의 분리 기준을 다르게 두었다고 생각할 수 있다. 기존에는 html, CSS, JS 파일로 뼈대, 스타일, 이벤트 등으로 관심사를 분리하였으나 styled component는 컴포넌트를 기준으로 분리했다고 할 수 있다. 컴포넌트의 스타일은 해당 컴포넌트 안에서 해결되도록 분리한 것이다. 기준을 다르게 했을 뿐 관심사를 분리한 것은 동일하다.

### 장점

- 컴포넌트를 기반으로 스타일링하기 때문에 각 컴포넌트에 대한 스타일을 쉽게 정의하고 유지보수 할 수 있다.
- 컴포넌트 범위 내에서만 적용되도록 보장하여 스타일 충돌을 방지한다.
- 동적으로 스타일을 변경하거나 조건부로 스타일을 적용하는 것이 용이하다.
- 스타일을 선언적으로 작성하여 가독성을 높이고, 스타일의 목적을 더 명확하게 표현할 수 있다.
- 최적화된 CSS를 생성하여 번들 크기를 줄일 수 있다.

### 단점

- 서버 사이드 렌더링을 사용하는 경우에는 CSS-in-JS의 사용이 복잡할 수 있으며, 초기 로딩 성능에 영향을 줄 수 있다.
- JavaScript가 비활성화되어 있는 경우에는 스타일이 적용되지 않을 수 있다.
- 추가적인 빌드 단계가 필요할 수 있으며, 빌드 시간이 늘어날 수 있다.
- 큰 규모의 프로젝트에서는 스타일을 관리하기가 어려울 수 있으며, 일관성 있는 스타일을 유지하기 위한 추가적인 노력이 필요할 수 있다.

## 사용법

`styled components`라는 npm 패키지명을 가지고 있어 npm 커맨드로 간단히 설치할 수 있다.

```bash
$ npm i styled-components
```

컴포넌트 상단에 `import` 예약어를 이용하여 불러온 뒤 사용할 수 있다.

```js
import styled from 'styled-components';
```

### styled-component 사용

CSS 스타일을 위해 컴포넌트를 선언하는 것은 아래와 같다. 백틱 안에 기존 CSS 문법을 사용하여 스타일을 적용한다.

```jsx
const StyledComponent = styled.div`
  <CSS 스타일 선언>
`;
```

그리고 해당 컴포넌트 안에서 선언한 styled component를 호출한다.

```jsx
function Component() {
  return (
    <>
      <StyledComponent />
    </>
  );
}
```

### props 사용

컴포넌트라는 이름과 같이 styled-component에서도 props를 활용할 수 있다.

```jsx
function Component() {
  return (
    <>
      <StyledComponent props={prop} />
    </>
  );
}

const StyledComponent = styled.div`
  color: ${({ prop }) => (prop ? 'black' : 'red')};
`;
```

props를 활용하여 상황에 따라 다른 스타일을 적용할 수 있다.

### class 사용

기존 CSS 가상 클래스나 styled-component 내에서 선언된 클래스를 활용하여 스타일을 달리 줄 수 있다.

```jsx
const StyledComponent = styled.div`
  $:focus {
    <CSS 스타일링>
  }

  $.className {
    <CSS 스타일링>
  }
`;
```

## 느낀점

위에서 정리한 것 같이 styled component의 장점은 스타일 충돌에 대한 걱정없이 사용할 수 있다는 것이 아주 컸다. 실제로 프로젝트 하는 내내 클래스 이름을 걱정하지 않아도 됐고, 내가 작성하지 않은 코드와의 스타일 충돌을 전혀 걱정하지 않고 진행했다. 하지만 완성된 컴포넌트의 모습을 보면 이게 공용 컴포넌트인지 styled component인지 한번에 확인이 어려웠다.

```jsx
function PostIdMessage() {
  return (
    <>
      <Header />
      <Container>
        <Section>
          <SectionTitle />
          <InputBox />
        </Section>
        <Section>
          <SectionTitle />
          <SelectBox />
        </Section>
        <Section>
          <SectionTitle />
          <QuillEditor />
        </Section>
        <Button />
      </Container>
    </>
  );
}
```

위는 내가 프로젝트에서 실제로 사용한 컴포넌트의 함축 코드이다. 위의 코드에는 공용 컴포넌트도 있고 styled-component도 존재한다. 어떠한 컴포넌트를 활용할 때 컴포넌트의 모양새나 props를 수정할 때가 있는데 이 때 해당 컴포넌트의 종류가 한번에 파악되지 않아 원본 컴포넌트를 찾아다니곤 했다. 이는 한 파일에 다른 코드가 있다는 것과는 다른 관점의 단점으로 생산성을 떨어트릴 수 있어서 다음 프로젝트때도 적용을 할 지는 고민해봐야 할 것 같다.
