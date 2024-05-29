---
date: '2024-04-11'
title: '[React] key가 필요한 이유'
categories: ['Note']
tags: ['React', 'WIL']
summary: '배열을 렌더링하고 싶은데요..'
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/8f78a4d1-d48b-4e52-acfd-bc670fd8f8fd'
---

<p align=center>
  <img src="https://github.com/Haze-S/blog-contents/assets/87344625/8f78a4d1-d48b-4e52-acfd-bc670fd8f8fd" alt="key 안쓰면 나는 에러">
</p>

배열을 렌더링할 때 key를 지정하지 않으면 위와 같은 경고가 나타난다. 실제로는 렌더링이 잘 이루어지지만 위와 같은 경고는 엘리먼트에 변화가 일어났을 때 예기치않은 상황이 발생하는 것을 예방하라는 이야기이다.

> “keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity".

리액트 공식 문서에 의하면 key는 리액트가 어떤 항목을 변경, 추가 또는 삭제할 지 식별하는 것을 돕고, 엘리먼트에 안정적인 고유성을 부여하기 위해 배열 내부의 엘리먼트에 지정해야 한다고 적혀있다.

즉, key는 데이터를 리렌더링 할 때 데이터에 부여된 식별자를 통해 해당 데이터가 변화된 것이 맞는 지 확인할 수 있게 해준다.

## key를 적용하는 방법

```jsx
function App() {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>
          <Component item={item} />
        </li>
      ))}
    </ul>
  );
}
```

key 값은 가장 바깥쪽에 있는 (최상위) 태그에다가 prop을 지정하면 된다. 보통 데이터의 고유한 값인 id를 많이 이용하지만 꼭 id일 필요는 없고 주민번호와 같은 고유한 값이면 무엇이든 가능하다.

## key를 쓰지 않으면 야기되는 상황

key를 사용하지 않으면 리액트가 요소의 고유성을 보장할 수 없다. 따라서 요소의 추가, 제거, 이동이 예상치 못한 결과를 초래할 수 있다. 예를 들어, 요소의 순서가 변경되면 리액트가 올바르게 업데이트하지 못할 수 있다.

```jsx
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([
    { id: 1, text: 'Apple' },
    { id: 2, text: 'Banana' },
    { id: 3, text: 'Orange' },
  ]);

  const removeItem = (idToRemove) => {
    setItems(items.filter((item) => item.id !== idToRemove));
  };

  return (
    <div>
      <h2>Shopping List</h2>
      <ul>
        {items.map((item) => (
          <li onClick={() => removeItem(item.id)}>{item.text}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

위 코드에서는 간단한 쇼핑 목록을 표시하고, 각 항목을 클릭하면 해당 항목이 제거되도록 구현되어 있다.

만약 사용자가 "Banana" 항목을 클릭하여 제거한다고 가정해보자. 이 때 "Banana" 항목은 배열에서 제거되고, "Orange" 항목이 두 번째로 이동한다.

리액트는 이 변경 사항을 감지할 수 있지만, 키를 사용하지 않았기 때문에 "Orange" 항목이 "Banana" 항목의 위치로 이동되었음을 인식하지 못 한다. 따라서 리액트는 "Orange" 항목을 삭제하고 새로운 "Orange" 항목을 배열의 끝에 추가하는 것으로 간주한다.

결과적으로 "Banana" 항목을 클릭하여 제거했지만, 실제로는 "Orange" 항목이 제거되어 예상치 못한 동작이 발생하게 된다. 이러한 문제는 키를 사용하여 각 요소를 고유하게 식별하고 추적할 때 해결할 수 있다.
