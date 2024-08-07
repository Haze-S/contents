---
date: '2024-05-06'
title: 'fetch를 사용한 이유'
categories: ['Note']
tags: ['JavaScript', 'Project', 'Rolling']
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/403a3b63-a3af-41c5-9ce9-895b67c803d2'
---

자바스크립트에서 api 리퀘스트를 보낼 때 사용되는 방법은 대표적으로 두 가지가 있다. 하나는 JS 안에 내장된 `fetch` 함수를 이용하는 것이고, 다른 하나는 `axios` 라이브러리를 사용하는 것이다. 이번 프로젝트에선 fetch 함수의 기본 동작과 비동기에 대해 익숙해지기 위해 라이브러리를 쓰지 않는 방향으로 진행했다.

## 어떤 걸 쓸까

![axios vs fetch](https://github.com/Haze-S/blog-contents/assets/87344625/505a45db-821c-4d4d-a51d-7ef9f070d1c5)

fetch와 axios의 문법은 크게 다르지 않다. 둘 다 프로미스 기반의 http 클라이언트여서 네트워크 요청 후에 결과를 담은 프로미스를 반환한다는 점은 동일하다. 차이점을 비교하자면 아래와 같다.

#### fetch

- 자바스크립트에 내장된 함수로서 별도의 설치나 import가 필요 없다.
- 내장 라이브러리이기 때문에 업데이트에 따른 에러 방지가 가능하다.
- 지원하지 않는 브라우저가 존재한다.
- 네트워크 에러 발생 시 response timeout이 없어 기다려야 한다.
- JSON으로 변환해주는 과정 필요하다.

#### axios

- response timeout 처리 방법이 존재한다.
- 크로스 브라우징 최적화로 브라우저 호환성이 뛰어나다.
- 사용을 위해 모듈 설치가 필요하다.

## fetch를 사용한 이유

사용한 이유는 여러가지가 있는데 나열해보자면 아래와 같다.

1. 롤링 프로젝트는 팀 프로젝트다.
   fetch의 경우 자바스크립트를 공부한 팀원들에겐 낯설지 않은 존재였고, HTTP 클라이언트 로직을 구현하는 사람은 나뿐이여서 모두에게 익숙한 것을 선택하는 것이 좋겠다 생각했다. api 함수 로직은 팀원 모두가 호출하게 될 함수이기 때문에 필요에 의해 함수 내용부를 확인하던가 수정할 경우, 코드를 이해하기 위해 axios 사용법을 찾는 시간을 쓰는 것은 생산성에 좋을 것 같진 않았다.
   <br/>
2. 추가적인 모듈 설치가 필요하지 않다.
   우리는 프로젝트를 진행하며 유달리 라이브러리들의 버전 충돌이 잦았다. 기능 구현을 위해 새로 라이브러리를 설치하는 경우 eslint의 버전때문에 라이브러리가 작동하지 않는 등 yarn.lock 파일에서도 충돌이 자주 일어났다. fetch의 경우 내장 함수라 추가 설치가 필요하지 않아서 프로젝트를 진행하며 더욱이 잘 선택했다 생각했다. 우리는 아직 모든 설정들을 자연스레 사용하지 못 하기 때문에..
   <br/>
3. 2주안에 완성될 수 있는 작은 프로젝트이다.
   해당 프로젝트는 배웠던 리액트를 활용하기 위한 팀 프로젝트였다. 따라서 기능이나 페이지의 수가 많지 않았고, api 또한 간단했다. 크로스 브라우징 이슈나 네트워크 에러 이슈까지는 생각하지 못 한건 맞지만, fetch와 프로미스 습득에 큰 의의를 두고 골랐다!

### axios 모방

fetch를 선택한 건 100% 나의 선택이여서 팀원들이 api 함수 사용을 최대한 편리하게 할 수 있도록 해야겠다 느꼈다.

- 직관적인 네이밍과 주석을 통해 함수와 매개 변수에 대한 설명도 빠짐없이 쓰려고 노력했다.
- JSON 데이터로 직렬화하는 것 또한 해당 함수에서 사용하도록 만들었다.
  - JSON 직렬화를 알아서 해주는 axios를 사용하지 않은 것 대신 함수를 호출하는 구간에서 직렬화에 대해 신경쓰지 않도록 api 함수 내에서 직렬화를 했다.

하지만 프로젝트를 진행하다보니 함수를 호출하는 곳에서 직렬화하는 것이 더 나은 상황일 수도 있겠다 생각이 들었다. 직렬화가 api 함수에서 진행되기 때문에 자바스크립트 순수 객체로 함수에 데이터를 전달하게 되는데 이로 인해 객체가 가진 특수성이 적용되는 일이 있었기 때문..

## 트러블 슈팅

정확히는 프로젝트 내에서는 async 함수를 이용해 api 로직을 구현했다. 비동기 함수안에서 fetch를 이용하여 콜백 지옥을 방지하고, 각각의 HTTP Method 마다 다른 함수를 선언하여 함수의 책임을 최소화했다.

![실제사용](https://github.com/Haze-S/blog-contents/assets/87344625/5a121011-8059-4e9a-b2d0-b0fdcab9868e)

#### 이슈

fetch를 이용하여 POST 요청을 보내니 `415(Unsupported Media Type)` 에러가 발생했다.
415 에러는 클라이언트와 서버의 요청/응답하는 데이터의 매개변수 설정이 잘못되었을 때 주로 발생하는 에러라는데 엔드 포인트도 틀린 게 없었고 메소드와 데이터의 형태 또한 다르지 않아서 당황했다.

#### 해결

에러가 발생한 원인은 HTTP 리퀘스트 메세지의 헤더 안에 있었다. axios의 경우 기본값이 `Content-Type: application/json` 인 반면에 fetch는 따로 정해져 있지 않다. 따라서 서버에 리퀘스트를 날릴 때 데이터를 json 형태로 보내달라는 명세서와 달리 기본 형태인 `Content-Type: application/x-www-form-urlencoded` 으로 데이터가 보내져 415 에러를 발생시켰다.

```js
const postRequest = async (data) => {
  const response = await fetch('api 주소', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
  });
};
```

이를 방지하기 위해 fetch 함수를 사용할 때 GET, DELETE 요청을 제외한, body에 데이터를 담아 리퀘스트를 날리는 모든 요청에는 두 번째 인자에 `Content-Type`을 명시해야 한다.

이런 이슈를 맞이하다보면 확실히 axios가 만들어지고 많이 사용되는 이유를 알 것 같다. 웹 서비스에서 대부분의 데이터 이동은 json으로 이루어지는데, axios는 확실히 json에 대해 개발자가 신경쓰지 않아도 되도록 한 것이 참 편리한 것 같다. 다음 프로젝트에는 axios 를 활용해서 진행해봐야겠다.

---

#### ref.

[javascript.info](https://ko.javascript.info/fetch#ref-351)
[[번역] 입문자를 위한 Axios vs Fetch](https://velog.io/@eunbinn/Axios-vs-Fetch)
