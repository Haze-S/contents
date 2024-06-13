---
date: '2024-06-12'
title: '[React] Deep-Dive Book study session 2'
categories: ['Study']
tags: ['Book', 'React']
summary: 'React Deep-Dive 북 스터디의 두번째, 챕터 5의 요약 및 설명을 맡았다.'
thumbnail: 'https://github.com/Haze-S/contents/assets/87344625/dc1b07e1-ee6b-495e-bfd9-82f03c993d69'
---

# 5장 리액트와 상태 관리 라이브러리

## 5.1 상태 관리는 왜 필요한가?

웹앱을 개발할 때 이야기하는 **"상태"** 는 어떠한 의미를 지닌 값이며 시나리오에 따라 지속적으로 변경될 수 있는 값을 의미한다. 또한 상호 작용이 가능한 모든 요소의 현재 값을 의미하기도 한다.

#### 대표적인 상태 분류

- UI
  - 다크/라이트 모드, 라디오를 비롯한 각종 input, 알림창의 노출 여부 등
- URL
  - 브라우저에서 관리되는 상태값
  - 쿼리 스트링을 이용한 상태가 존재하며 사용자의 라우팅에 따라 변경된다.
- 폼(form)
  - loading, submit, disabled, validation 등
- 서버 데이터
  - 클라이언트에서 요청을 통해 서버에서 가져온 값
  - 대표적으로 API 요청이 있다.

상태 관리 자체는 크게 어려운 일이 아니며, 단순히 손이 많이 가는 문제일 수도 있다. 하지만 애플리케이션 전체적으로 관리해야할 상태가 있다고 가정하고 그 상태에 맞는 다양한 UI 요소를 보여줘야 하는 상황이라면 상태는 어디에 둘 지, 유효한 범위는 어떻게 제한할 지, tearing 현상을 어떻게 방지할 지 등 많은 고민을 하게 될 것이다. 이처럼 현대 웹앱에서는 상태를 효율적으로 관리하고 필요한 쪽에서는 빠르게 반응할 수 있는 모델에 대한 고민이 본격적으로 시작되었다.

### 5.1.1 리액트 상태 관리의 역사

타 프레임워크와 달리 리액트는 사용자 인터페이스를 만들기 위한 라이브러리이고 그 이상의 기능을 제공하지 않고 있다. 따라서 상태를 관리하는 방법도 개발자에 따라, 시간에 따라 많은 차이가 발생했다.

#### Flux 패턴

2014년, 리액트의 등장과 비슷한 시기에 Flux 패턴과 함께 이를 기반으로 한 Flux 라이브러리가 생긴다. 이 당시 웹 개발 상황은 웹앱이 커지고 상태(데이터)도 많아짐에 따라 해당 상태의 변화를 추적하고 이해하기 어려운 상황이었다.

![flux-mvc](https://github.com/Haze-S/contents/assets/87344625/d1418544-d76e-4ade-9a4b-b85a0219f870)

이에 페이스북 팀은 문제의 원인을 양방향 데이터 바인딩으로 보았고, 이를 해결하기 위해 데이터 흐름을 단방향으로 변경하는 것을 제안했다. 이것이 바로 Flux 패턴의 시작이다.

##### 용어 정의

- 액션(action)
  - 어떠한 작업을 처리할 액션과 포함시킬 데이터를 의미한다.
  - 타입과 데이터를 정의해 디스패처로 보낸다.
- 디스패처(dispatcher)
  - 액션을 스토어에 보내는 역할
  - 콜백 함수 형태로 액션이 정의한 타입과 데이터를 스토어에 보낸다.
- 스토어(store)
  - 실제 상태에 따른 값과 상태를 변경할 수 있는 메서드를 가진다.
  - 액션 타입에 따라 어떻게 이를 변경할 지 정의되어 있다.
- 뷰(view)
  - 리액트의 컴포넌트에 해당
  - 스토어에서 만들어진 데이터를 가져와 화면을 렌더링한다.
  - 뷰에서 사용자의 행위에 따라 상태를 업데이트 할 경우 액션을 호출하는 구조로 구성된다.

![flux-one-way](https://github.com/Haze-S/contents/assets/87344625/b577d995-5e3a-4674-bd86-954f0739546b)

단방향 데이터 흐름 방식은 사용자 입력에 따라 일일히 데이터를 갱신하고 화면을 업데이트 하는 코드를 짜야하므로 코드의 양이 많아지고 개발자가 수고스러워지는 불편함도 존재한다. 다만 데이터의 흐름이 액션이라는 한 방향으로 줄어드므로 흐름을 추적하기 쉽고 코드를 이해하기 수월해진다.

리액트는 대표적인 단방향 데이터 바인딩을 기반으로 한 라이브러리이므로 Flux 패턴과 궁합이 잘 맞았고 이런 패턴을 따르는 다양한 라이브러리들이 생겨났다.

#### 시장 지배자 리덕스의 등장

리덕스도 최초에는 Flux 구조를 구현하기 위해 만들어진 라이브러리였다. 한 가지 특별한 것은 여기에 Elm 아키텍처를 도입했다는 것이다. Elm은 웹페이지를 선언적으로 작성하기 위한 언어이다.

```elm
module Main exposing(..)

import Browser
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)

-- MAIN
main =
  Browser.sandbox { init = init, update = update, view = view }

-- MODEL
type alias Model = Int

init : Model
init =
  0

-- UPDATE
type Msg
  = Increment
  | Decrement

update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1

-- VIEW
view : Model -> Html Msg
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ],
      div [] [ text (String.fromInt model) ],
      button [ onClick Increment ] [ text "+" ]]
<div>
  <button>-</button>
  <div>2</div>
  <button>+</button>
</div>
```

위의 코드는 Elm으로 HTML을 작성한 예제이다. 여기서 주목할 것은 model, update, view 이며 이 세 가지가 Elm 아키텍처의 핵심이다.

- 모델(model) : 애플리케이션의 상태
- 뷰(view) : 모델을 표현하는 HTML
- 업데이트(update) : 모델을 수정하는 방식

Elm은 Flux와 마찬가지로 데이터 흐름을 세 가지로 분류해 이를 단방향으로 강제하여 상태를 안정적으로 관리하고자 했다. 그리고 리덕스는 Elm 아키텍처의 영향을 받았다.

리덕스는 하나의 상태 객체를 스토어에 저장해 두고 업데이트 하는 작업을 디스패치해 업데이트를 수행한다. 이런 작업은 `reducer` 함수로 발생시키는데 이 함수는 상태에 대해 완전히 새로운 복사본을 반환하고 애플리케이션에 새롭게 만들어진 상태를 전파한다. 리덕스의 등장으로 props drilling 을 해결할 수 있었고 스토어가 필요한 컴포넌트는 `connect` 만으로 스토어에 접근이 가능했다. Context API가 등장한 이후로도 리덕스는 상태 관리에서 중요한 축으로 자리 잡았다.
하지만 이러한 리덕스도 초기에는 하려는 일에 비해 보일러플레이트가 너무 많다는 비판의 목소리가 있었다. 단순히 하나의 상태만 바꾸려해도 액션 타입을 선언해야 하고 `creator` 함수도 만들어야 했고 dispatcher와 selector, 기존 리듀서 내부의 변경 방식 등을 정의해야 했다.

#### Context API 와 useContext

리액트 팀은 리액트 16.3에서 props drilling을 해결하기 위해 전역 상태를 하위 컴포넌트에 주입할 수 있는 Context API를 출시했다. 이는 props로 상태를 넘겨주지 않아도 원하는 곳에서 Context Provider가 상태를 주입하여 사용할 수 있게 되었다.
이전에도 context가 존재하여 `getChildContext()` 메서드를 제공했지만 이 방식에는 문제점이 있었다.

1. 상위 컴포넌트가 렌더링되면 `getChildContext` 도 호출되어 동시에 `shouldComponentUpdate` 가 항상 true 를 반환, 불필요한 렌더링이 일어났다.
2. `getChildContext` 를 사용하기 위해선 `context` 를 인수로 받아야 하는데 이 때문에 컴포넌트와 결합도가 높아졌다.

위와 같은 단점을 해결하기 위해 새로운 context가 출시됐다.

```jsx
const Context = (createContext < Type) | (undefined > undefined);

export default class MyApp extends Component<{}, Type> {
  render() {
    return (
      <Context.Provider value={this.value}>
        <button onClick={this.handleClick}>+</button>
        <DummyParent />
      </Context.Provider>
    );
  }
}
```

하지만 Context API는 상태 관리가 아닌 주입을 도와주는 기능이며 렌더링을 막는다던가 하는 방법은 없으니 사용할 때 주의가 필요하다.

#### 훅의 탄생, 그리고 React Query 와 SWR

이후 리액트는 16.8 버전에서 함수 컴포넌트와 다양한 훅 API를 추가했다. 훅 API들은 함수 컴포넌트의 인기를 높혀줄 많은 기능들을 제공했고 이 중 가장 큰 변경점 중 하나는 **state를 재사용하기 매우 쉬워졌다** 는 것이다.

```js
function useCounter() {
  const [count, setCount] = useState(0);

  function increase() {
    setCount((prev) => prev + 1);
  }

  return { count, increase };
}
```

위의 예제는 count state 를 1씩 올려주는 custom hook이다. 이러한 방법은 state를 내부적으로 관리하고 다른 곳에서 재사용할 수 있게 되었다. 이는 클래스 컴포넌트보다 훨씬 간결하고 직관적인 방법이었다.

이러한 hook과 state의 등장으로 이전과는 다른 방식의 상태 관리 라이브러리인 **React Query 와 SWR** 이 등장한다. 두 라이브러리는 모두 외부에서 데이터를 불러오는 fetch를 관리하는 데 특화된 라이브러리이지만 **API 호출에 대한 상태를 관리하기 때문에 HTTP 요청에 특화된 상태 관리 라이브러리** 라 볼 수 있다.

```jsx
import useSWR from 'swr';

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function App() {
  const { data, error } = useSWR(
    'https://api.github.com/repos/vercel/swr',
    fetcher
  );

  if (error) return 'An error has occurred';
  if (!data) return 'Loading..';

  return (
    <div>
      <p>{JSON.stringify(data)}</p>
    </div>
  );
}
```

위의 코드는 SWR을 사용한 예제로 `useSWR` 의 첫 번째 인수로 조회할 API 주소를, 두 번째 인수로 조회에 사용되는 fetch를 넘겨준다. API 주소는 키로도 사용되며 다른 곳에서 동일한 키로 호출하면 다시 조회하지 않고 `useSWR` 이 관리하는 캐시의 값을 활용한다. 기존의 상태 관리 라이브러리보단 제한적인 목적을 가지고 있고 형태도 다르지만 둘은 산태 관리 라이브러리의 일종이라 볼 수 있다.

#### Recoil, Zustand, Jotai, Valtio에 이르기까지

좀 더 범용적으로 쓸 수 있는 상태 관리 라이브러리들은 훅의 등장에 따라 훅을 활용해 상태를 가져오거나 관리하는 다양한 라이브러리들이 등장한다.

```jsx
// Recoil
const counter = atom({ key: 'count', default: 0 });
const todoList = useRecoilValue(counter);

// Jotai
const countAtom = atom(0);
const [count, setCount] = useAtom(countAtom);

//Zustand
const useCounterStore = create((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
}));
const count = useCounterStore((state) => state.count);

// Valtio
const state = proxy({ count: 0 });
const snap = useSnapshot(state);
state.count++;
```

기존 리덕스 같은 라이브러리와는 다르게 훅을 활용해 작은 크기의 상태를 효율적으로 관리한다. 위 예제에서 사용된 라이브러리들은 모두 리액트 16.8 버전 이상을 요구한다. 리덕스나 Mobx도 다른 라이브러리를 사용하면 훅으로 상태를 가져올 수 있긴하지만 위의 라이브러리들은 별도로 설치 없이 사용 가능하다. 이들은 개발자가 원하는 만큼의 상태를 지역적으로 관리할 수 있게 했고 훅을 지원함으로 함수 컴포넌트에서 손쉽게 사용할 수 있게 했다.

## 5.2 리액트 훅으로 시작하는 상태 관리

오랜 기간동안 상태 관리를 위해 리덕스에 의존했으나 현재는 `Context API`, `useReducer`, `useState` 를 사용하여 상태를 관리하는 방법들이 등장했다.

### 5.2.1 가장 기본적인 방법: useState 와 useReducer

`useState` 의 등장으로 리액트에선 여러 컴포넌트에 걸쳐 동일한 인터페이스의 상태를 손쉽게 생성하고 관리할 수 있게 됐다. 리액트의 훅을 기반으로 만든 커스텀 훅은 함수 컴포넌트라면 어디서든 재사용이 가능하다.

`useReducer` 또한 지역 상태를 관리할 수 있는 훅이다. 실제로 `useState` 는 `useReducer` 로 구현되었으며 `useReducer` 또한 `useState` 로 작성할 수 있다. 참고로 `useReducer` 를 타입스크립트로 작성하려면 다양한 형태의 오버로딩이 필요하다. 두 훅 모두 구현상의 차이만 있을 뿐 지역 상태 관리를 위해 만들어졌다.

하지만 `useState` 와 `useReducer` 가 상태 관리의 모든 필요성과 문제를 해결해 주지는 않는다. 이 둘을 기반으로 하는 커스텀 훅의 한계는 명확한데, 훅을 사용할 때마다 컴포넌트별로 초기화되므로 컴포넌트에 따라 서로 다른 상태를 가질 수 밖에 없다는 것이다. 이렇게 기본적인 **`useState` 를 기반으로 한 상태를 지역상태(local state)** 라고 하며 지역 상태는 **해당 컴포넌트 내에서만 유효하다** 는 한계가 있다.

전역 상태로 만들어 컴포넌트가 사용하는 모든 훅이 동일한 값을 참조할 수 있게 하려면 커스텀 훅을 각 컴포넌트에서 사용하지 않고 상위 컴포넌트에서만 사용하고, 훅의 반환값을 하위 컴포넌트의 props로 제공하는 방법이 있다. 즉, 지역 상태인 커스텀 훅을 부모 컴포넌트로 한 단계 끌어올린 다음 이 값을 하위 컴포넌트에서 참조해 재사용하게 만드는 것이다. 이는 props 형태로 제공해야 한다는 점이 불편하고 컴포넌트 트리를 재설계하는 수고스러움이 발생한다.

### 5.2.2 지역 상태의 한계를 벗어나보자: useState의 상태를 바깥으로 분리하기

한계가 명확한 `useState` 는 리액트가 만든 클로저 내부에서 관리되어 지역 상태로 생성되기 때문에 해당 컴포넌트에서만 사용할 수 있다는 단점이 있다. 만약 `useState` 가 리액트 클로저가 아닌 완전히 다른 곳에서 초기화되어 관리된다면 어떨까? 즉, 별개의 파일에서 해당 값을 업데이트하면 그 값을 참조하고 있는 컴포넌트나 훅에서도 해당 값을 사용할 수 있지 않을까? 아쉽게도 이러한 방식은 리액트 환경에서 작동하지 않는다. 원인은 리액트 렌더링 방식 때문인데 별개의 파일을 생성해서 상태를 컴포넌트 밖에 선언하게 되면 리렌더링을 일으키는 장치가 존재하지 않기 때문이다.

즉, 상태를 업데이트하는 것뿐만 아니라 업데이트됐을 때 컴포넌트에 반영시키기 위한 리렌더링이 필요하며 함수 컴포넌트에서 리렌더링을 하려면 아래와 같은 작업 중 하나가 발생해야 한다.

- 컴포넌트 렌더링과 관계없는 직접적인 상태를 관리하지 않더라도 `useState`, `useReducer` 의 반환값 중 두 번째 인수가 호출된다.
- 부모 컴포넌트가 리렌더링되거나 해당 함수가 다시 실행되어야 한다.

`useState` 로 컴포넌트의 리렌더링을 실행해 최신값을 가져오는 방법은 해당 컴포넌트 자체에서만 유효한 전략으로 다른 컴포넌트에서는 리렌더링을 일으킬 무언가가 없기 때문에 렌더링이 되지 않는다.

위와 같은 한계를 종합하면 함수 외부에서 상태를 참조하고 이를 통해 렌더링까지 이뤄지려면 아래와 같은 조건을 만족해야 한다.

1. 컴포넌트 외부 어딘가에 상태를 두고 여러 컴포넌트가 같이 쓸 수 있어야 한다.
2. 외부에 있는 상태를 사용하는 컴포넌트는 변화를 알아채고 변화할 때마다 리렌더링이 일어나 최신 상태값 기준으로 렌더링해야 한다. 상태 감지는 상태를 참조하는 모든 컴포넌트에서 동일하게 작동해야 한다.
3. 상태가 객체인 경우 감지하지 않는 값은 변한다 하더라도 리렌더링이 발생해선 안 된다.

```jsx
function Counter() {
  const subscription = useMemo(
    () => ({
      // 스토어의 모든 값으로 설정해 뒀지만 특정한 값에서만 가져오는 것도 가능하다.
      getCurrentValue: () => store.get(),
      subscribe: (callback: () => void) => {
        const unsubscribe = store.subscribe(callback);
        return () => unsubscribe();
      },
    }),
    []
  );
  const value = useSubscription(subscription);

  return <>{JSON.stringify(value)}</>;
}
```

페이스북 팀에서 만든 `useSubscription` 훅은 위의 조건을 만족시켜 리액트 외부에서 관리되는 값에 대한 변경을 추적하고 이를 리렌더링까지 할 수 있는 훅이다. 리액트 18 버전의 해당 훅은 `useSyncExternalStore` 로 재작성 되어있다.

### 5.2.3 useState와 Context를 동시에 사용해보기

커스텀 훅을 활용해 `useState` 로 관리하지 않는 외부 상태값을 읽어보고 리렌더링까지 사용하는 방법을 알아봤는데 이는 하나의 단점이 있다. 해당 훅과 스토어를 사용하는 구조는 반드시 하나의 스토어만 가지게 된다는 것이다. 해당 스토어는 마치 전역 변수처럼 작동하여 동일한 형태의 여러 개의 스토어를 가질 수 없게 된다. 훅을 사용하는 서로 다른 스코프에서 스토어의 구조는 동일하되 서로 다른 데이터를 공유하려면 어떻게 할까?

단순히 `createStore` 를 여러개 만드는 방식은 스토어가 필요할 때마다 반복적으로 생성하게 하고 훅이 스토에와 1:1로 의존적인 관계를 맺고 있게 된다. 이러한 문제는 `Context` 로 해결할 수 있다. `Context` 를 활용해 해당 스토어를 하위 컴포넌트에 주입한다면 컴포넌트는 주입된 스토어에 대해서만 접근할 수 있게 된다. `Context` 와 `Provider` 를 기반으로 각 store 값을 격리해서 관리하면 아래와 같은 장점이 있다.

- 스토어를 사용하는 컴포넌트는 상태가 어느 스토어에서 온 건지 신경 쓰지 않아도 된다.
- 단지 해당 스토어를 기반으로 어떤 값을 보여줄지만 고민하면 된다.
- 부모 컴포넌트 입장에서 자식 컴포넌트에 따라 보여주고 싶은 데이터를 `Context` 로 잘 격리하기만 하면 된다.
- 부모와 자식의 책임과 역할을 명시적인 코드로 나눌 수 있다.

### 5.2.4 상태 관리 라이브러리 Recoil, Jotai, Zustand 살펴보기

Recoil과 Jotai는 Context와 Provider, 그리고 훅을 기반으로 가능한 작은 상태를 효율적으로 관리하는 데 초점을 맞추고 있으며 Zustand는 리덕스와 같이 하나의 큰 스토어를 기반으로 상태를 관리하는 라이브러리이다. 이는 스토어가 가지는 클로저를 기반으로 생성되어 스토어 상태가 변하면 상태를 구독하는 컴포넌트에 전파해 리렌더링을 알린다.

#### 페이스북이 만든 상태 관리 라이브러리 Recoil

Recoil은 훅의 개념으로 상태 관리를 시작한 최초의 라이브러리 중 하나이며 최소 상태 개념인 `Atom` 을 처음 리액트 생태계에 선보이기도 했다. 아직 정식으로 출시되지 않은 라이브러리로 2023년 3월에 0.7.7 버전에서 멈춰있다. Recoil 팀에서는 리액트 18에서 제공되는 동시성 렌더링, 서버 컴포넌트, Streaming SSR 등이 지원되기 쩐까지 1.0.0을 릴리스하지 않을 것이라 밝힌 적 있다. 따라서 Recoil은 실제 프로덕션에서 안정성이나 성능, 사용성 등을 보장할 수 없으며 유의적 버전에 따라 부 버전이 변경돼도 호환성이 깨지는 변경 사항이 발생할 수도 있는 위험이 있다. 책에서는 22년 9월 기준으로 0.7.5 버전에 대해 설명했다.

##### RecoilRoot

Recoil을 사용하기 위해서는 `RecoilRoot`를 앱 최상단에 선언해야 한다. `RecoilRoot`의 용도는 Recoil에서 생성되는 상태값을 저장하기 위한 스토어를 생성한다.

`useStoreRef` 가 가리키는 것은 AppContext가 가지고 있는 스토어로 atom과 같은 상태값을 저장하는 스토어를 의미한다.

스토어를 살펴보면 아이디 값을 가져오는 `getNextStoreID()` 와 값을 가져오는 `getState`, 값을 수정하는 `replaceState` 등으로 이루어져 있다. 해당 스토어 아이디를 제외하고는 모두 에러로 처리되는데 이는 `RecoilRoot` 로 감싸지 않은 컴포넌트는 스토어에 접근할 수 없다는 것을 의미한다. 추가로 `notifyComponents()` 함수는 store와 storeState를 인수로 받아 해당 스토어를 사용하는 하위 의존성을 모두 검색하여 거기에 있는 컴포넌트들을 모두 확인해 콜백을 실행한다. 값이 변경됐을 때 콜백을 실행해 상태 변화를 알린다는 것은 바닐라 스토어와 크게 다르지 않다.

`RecoilRoot` 의 구조를 파악해보면 아래와 같은 특징이 있다는 것을 알 수 있다.

- Recoil의 상태값은 RecoilRoot로 생성된 Context의 스토어에 저장된다.
- 스토어의 상태값에 접근할 수 있는 함수들이 있으며 이 함수를 사용해 상태값에 접근하거나 변경한다.
- 값의 변경이 발생하면 이를 참조하는 하위 컴포넌트에 모두 알린다.

##### Atom

atom은 상태를 나타내는 Recoil의 최소 상태 단위이다. key값을 필수로 가지며 해당 Key는 다른 atom과 구별하는 식별자가 되는 필수 값이다. 키는 앱 내부에서 유일한 값이어야 하기 때문에 atom과 selector를 만들 때 주의를 기울여야 하며 default는 atom의 초기값을 의미한다.

- `useRecoilValue`
  - atom의 값을 읽어오는 훅
- `useRecoilState`
  - `useState` 와 유사하게 값을 가져오고 변경할 수 있는 훅

atom에 비동기 작업도 추가할 수 있으며 `useRecoilStateLoadable`, `waitForAll`, `waitForAny`, `waitForAllSettled` 와 같이 비동기 작업을 지원하는 API도 존재한다.

##### 특징

- 간단한 API
  - Recoil은 간단하고 직관적인 API를 제공하여, 상태 관리를 쉽게 시작할 수 있다.
  - atom과 selector라는 두 가지 주요 개념을 사용하여 상태와 파생 상태를 정의한다.
- 컴포넌트 기반의 상태 관리
  - Recoil은 상태를 컴포넌트 트리와 자연스럽게 통합하여, 컴포넌트 기반의 상태 관리를 지원한다.
- 비동기 상태 관리
  - Recoil의 selector는 비동기 데이터를 쉽게 다룰 수 있게 해준다.
- Recoil Root
  - Recoil 상태는 RecoilRoot 컴포넌트 안에서 관리된다.
- 파인 그레인드 업데이트
  - Recoil은 상태의 일부가 변경될 때 필요한 컴포넌트만 다시 렌더링한다. 이는 불필요한 렌더링을 줄이고 성능을 최적화하는 데 도움을 준다.
- 디버깅 및 개발 도구
  - Recoil은 개발 도구를 통해 상태 변화를 추적하고 디버깅할 수 있다. Recoil DevTools를 사용하면 상태의 변경 내역을 시각적으로 확인할 수 있다.
- 동시성 제어
  - Recoil은 React의 concurrent 모드와 잘 작동하며, 동시성 제어 기능을 제공한다. 이는 최신 React 기능과의 호환성을 보장한다.
- React Hooks 사용
  - Recoil은 React의 훅(Hooks)을 활용하여 상태를 관리한다.

#### Recoil에서 영감을 받은, 그러나 조금 더 유연한 Jotai

Recoil의 atom 모델에 영감을 받아 만들어진 라이브러리로 상향식(bottom-up) 접근법을 취하고 있다. 이는 작은 단위의 상태를 위로 전파할 수 있는 구조로 Context의 문제점인 불필요한 리렌더링이 일어나는 문제를 해결하고자 설계됐고 또한 개발자들이 메모이제이션이나 최적화를 거치지 않아도 리렌더링이 발생하지 않도록 되어있다. 책에서는 22년 9월 기준 버전 1.8.3을 설명했다.

##### atom

Recoil과 마찬가지로 최소 단위 상태를 의미한다. atom 하나로 상태를 만들수도 있고 Recoil과 다르게 파생된 상태를 만들 수도 있다. Jotai의 atom은 별도의 key를 넘겨주지 않아도 되며 내부에 있는 key 변수는 단지 `toString()` 을 위한 용도로 한정되어 있다. 또한 config 객체를 반환하는데 이는 초기값을 의미하는 `init`, 값을 가져오는 `read`, 값을 설정하는 `write` 가 존재한다. 즉, Jotai에서의 atom은 상태를 따로 저장하지 않는다.

##### useAtomValue

`useAtomValue` 의 내부 코드를 보면 `useReducer` 가 존재하는데, 여기서 반환하는 상태값은 store의 버전, atom에서 get을 수행했을 때 반환값, atom 그 자체를 의미하는 3가지로 구성되어 있다. Recoil과 다르게 컴포넌트 루트 레벨에서 Context가 없어도 되며, Context가 없다면 Provider가 없는 형태로 기본 스토어를 루트에 생성하고 이를 활용해 값을 저장한다.

atom의 값은 store에 존재하며 atom 객체 자체를 키로 활용해 값을 저장한다. 그리고 리렌더링을 일으키기 위해 `rerenderIfChanged` 를 사용하는데 아래와 같은 경우에 `rerenderIfChanged` 가 실행된다.

1. 넘겨받은 atom이 Reducer를 통해 스토어에 있는 atom과 달라지는 경우
2. subscribe을 수행하고 있다가 어디선가 이 값이 달라지는 경우

위와 같은 로직 덕분에 atom의 값이 어디서 변경되더라도 `useAtomValue` 로 값을 사용하는 쪽에서는 언제든 최신 값의 atom을 사용해 렌더링 할 수 있다.

##### useAtom

useState와 동일한 형태의 배열을 반환한다. 해당 훅은 atom을 수정할 수 있는 기능을 제공한다. 구현부의 `write` 함수가 스토어에서 해당 atom을 찾아 직접 값을 업데이트하고 이후에 `listener` 함수를 실행해 값의 변화가 있음을 전파한다.

Jotai의 atom API는 컴포넌트 외부에서도 선언할 수 있으며 값뿐만 아니라 함수를 인수로 받을 수 있다. 이러한 특징을 활용해 atom의 값으로부터 파생된 atom을 만드는 것이다.

##### 특징

- 비교적 간결한 API
  - Recoil의 atom은 별도의 키를 관리해야 하는 반면 Jotai는 관리할 필요가 없다.
  - Recoil은 파생된 값을 만들기 위해 selector가 필요했지만 Jotai는 atom 만으로 파생된 상태를 만들 수 있다.
- 작은 번들 크기
  - Jotai는 매우 가벼운 라이브러리로, 번들 크기가 작아 애플리케이션의 성능에 미치는 영향이 적다.
- TypeScript 지원
  - Jotai는 TypeScript를 완벽하게 지원하여, 타입 안전성을 보장하고 개발 중 오류를 줄일 수 있다.
- React Concurrent Mode 지원
  - Jotai는 React의 Concurrent Mode를 지원하여, 최신 React 기능과 호환된다.

#### 작고 빠르며 확장에도 유연한 Zustand

Zustand는 리덕스에 영감을 받아 만들어졌으며 하나의 스토어를 중앙 집중형으로 활용해 스토어 내부에서 상태를 관리한다. 책에서는 22년 9월 기준의 4.1.1버전을 설명했다.

##### Zustand 바닐라 코드

Zustand의 스토어 구조는 state의 값은 useState 외부에서 관리를 한다. 한 가지 특이한 것은 `partial` 과 `replace` 인데 `partial` 은 state의 일부분만 변경하고 싶을 때 사용하고 `replace` 는 state를 완전히 새로운 값으로 변경할 때 사용한다. 이를 통해 state 값이 객체일 때 필요에 따라 나눠서 사용할 수 있을 것으로 보인다.

추가로 `subscribe` 는 `listener` 를 등록하고 `listener` 는 Set 형태로 선언되어 추가, 삭제, 중복 관리가 용이하게 설계되어있다. 즉, 상태값이 변경될 때 리렌더링이 필요한 컴포넌트에 전파될 목적으로 만들어졌다.

스토어 코드가 있는 파일에서 유일하게 `export` 하는 함수 및 변수는 `createStore` 이며 어떤 것도 `import` 하지 않는 것을 알 수 있다. 즉 그 어떤 프레임워크와는 별개로 완전히 독립적으로 구성되어 있음을 알 수 있다.

##### Zustand의 리액트 코드

Zustand를 리액트에서 사용하기 위해서는 어디선가 store를 읽고 리렌더링을 해야한다. 리액트에서 사용할 수 있도록 도와주는 함수들은 `./src/react.ts` 에서 관리된다. `export` 하는 것은 해당 스토어를 사용할 수 있는 `useStore` 함수와 새로 스토어를 만드는 `create` 변수가 있으며 이 둘을 이용하면 리액트에서 스토어를 사용할 수 있다.

##### 간단한 사용법

```jsx
import { create } from 'zustand';

const useCounterStore = create((set) => ({
  count: 1,
  inc: () => set((state) => ({ count: state.count + 1 })),
  dec: () => set((state) => ({ count: state.count - 1 })),
}));

function Counter() {
  const { count, inc, dec } = useCounterStore();
  return (
    <div class="counter">
      <span>{count}</span>
      <button onClick={inc}>up</button>
      <button onClick={dec}>down</button>
    </div>
  );
}
```

Zustand의 `create` 를 사용해 스토어를 만들고 반환 값으로 스토어를 컴포넌트 내부에서 사용할 수 있는 훅을 생성했다.

```jsx
import { createStore, useStore } from 'zustand'

const counterStore = createStore((set) => ({
  ...
}))

function Counter() {
  const { count, inc, dec } = useStore(counterStore)
  ...
}
```

위와 같은 기능을 하는 예제로 리액트 컴포넌트 외부에 store를 만드는 방법이다. `createStore` 를 사용하면 리액트와 상관없는 바닐라 스토어를 만들 수 있으며 해당 스토어는 `useStore` 훅을 통해 접근하여 리액트 컴포넌트 내부에서 사용할 수 있게 된다.

##### 특징

- 접근성
  - 많은 코드를 작성하지 않아도 빠르게 스토어를 만들고 사용할 수 있다. 이는 리덕스와 확실히 구별되는 특징이다.
  - API가 복잡하지 않고 사용이 간단해 초보자들도 쉽게 접근할 수 있는 라이브러리이다.
- 작은 번들 크기
  - Bundlephobia 기준으로 크기가 고작 2.9kB밖에 되지 않는다.
- 타입스크립트 지원
  - 타입스크립트 기반으로 작성되어 별도로 `@type` 을 설치하거나 `d.ts` 에 대한 우려 없이 타입스크립트를 쓸 수 있다.
- 미들웨어 지원
  - `create` 의 두 번째 인수로 미들웨어를 추가할 수 있다.
  - 여러 기능을 하는 미들웨어를 다양하게 제공하여 추가적인 작업을 정의할 수 있게 한다.

### 5.2.5 정리

각 라이브러리가 상태를 관리하는 방식은 차이가 있지만 리렌더링을 일으키키 위한 방식은 제한적이여서 어떤 방식을 채택하든지 리렌더링을 만드는 방법은 거의 동일하다. 따라서 각 라이브러리의 특징을 파악하고 현재 앱의 상황과 철학에 맞는 것을 선택하는 것이 효율적인 상태 관리가 가능할 것이다. 또한 메인테이너가 많고 다운로드가 활발하며 관리가 잘되고 있는 라이브러리를 선택하는 것이 좋다.
