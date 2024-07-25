---
date: '2024-06-25'
title: 'reduce와 재귀 함수를 이용한 데이터 중복 제거'
categories: ['Note']
tags: ['Project', 'Todo-Todo']
thumbnail: 'https://github.com/user-attachments/assets/c204cf99-cf8b-45f4-89a8-900b152406be'
---

## 대시보드 중복 데이터 제거

이번 프로젝트를 시작하면서 페이지네이션 기능을 맡아서 하게됐는데 대시보드 데이터에서 치명적인 오류가 발생했다. 대시보드 데이터에 대한 기본적인 요구 사항은 아래와 같다.

- 사용자가 로그인 시 `/mydashboard` 페이지로 이동되며, 자신이 포함된 대시보드 목록이 화면에 나타난다.
- 대시보드 데이터는 사이드바에도 렌더링되며 페이지네이션 기능이 구현되어 버튼을 누를 때 마다 페이지가 이동된다.
- 데이터 GET 요청 시 페이지 번호를 넘겨 필요한 만큼의 데이터를 받아온다.

### 발생한 문제

<div align=center>

![해결 전 페이지 화면](https://github.com/user-attachments/assets/203d2af6-6f5e-45b8-9f58-c97afab1e8c3)

</div>

![key값 에러](https://github.com/user-attachments/assets/14a450d6-ac1d-483c-a5db-b7653a428e8a)

패칭한 데이터를 매핑하던 중 특정 아이디에서만 계속해서 대시보드 데이터의 키값이 중복되는 현상이 발생했다. 모든 유저에게 나타난다면 이해가 될텐데 하나의 유저에게만 이런 현상이 나타나서 당황했다. 데이터를 매핑할 때 사용한 키값은 응답 데이터로 넘어오는 대시보드의 고유한 id값을 이용했다.

### 문제 확인

처음에는 대시보드 데이터 양의 문제인가 생각했다. 하지만 전체 데이터를 패칭하는 것도 아니고, 나눠서 받아오는 데이터들을 화면에 렌더링할 뿐인데 양이 왜 문제가 되지..? 코드에 이상이 있었다면 동일하게 페이지네이션 기능을 사용하는 멤버 데이터에도 똑같이 이런 현상이 발견되어야 하는데 그렇지 않았다. 정확한 이유를 알기 위해 스웨거를 통해 페이지 넘버와 데이터 사이즈, 유저를 바꿔가며 GET 요청을 보내 응답 데이터를 확인했고 key값이 중복되는 이유를 알게 되었다.

![스웨거 응답값](https://github.com/user-attachments/assets/e7819978-4929-4aae-9635-c8e89ea569ce)

앞서 나는 데이터를 매핑할 때 응답 데이터에 담긴 대시보드 id를 key로 사용했고, 응답으로 오는 데이터 자체가 중복되어 왔기 때문에 당연히 key값이 중복될 수 밖에 없었다. 이런 현상이 발생한 이유는 서버에서 api 설계할 때 대시보드 멤버를 중복으로 초대할 수 있게끔 설계한 것에서 파생된 문제였다.

- 대시보드 멤버를 중복 초대할 수 있기 때문에 당연히 중복 수락된 대시보드 데이터가 발생
- 대시보드 목록 데이터를 응답으로 보낼 때 당연히 같은 id를 가진 대시보드가 응답으로 보내짐
- 단순히 GET 요청을 보낸 후 목록을 매핑할 시 중복된 key값으로 인해 매핑이 올바르게 이뤄지지 않음

프로젝트 기간은 2주였고, api 수정은 바로 이뤄질 수 없는 상황이여서 나는 클라이언트 단에서 해결하기로 했다.

### 해결 과정

이 문제를 클라이언트에서 해결하려면 어쩔 수 없는 네트워크 비용이 들어간다. api 요청 시 페이지 번호와 데이터 사이즈를 함께 보내 GET 하는데, 모든 데이터를 받아와서 처리하기엔 중복이 없는 유저의 경우엔 굳이 그럴 필요없이 페이지에 따라 필요한 데이터만 받아오면 되고 중복이 있다쳐도 모든 데이터를 받아올 땐 그만큼의 시간이 필요하게 된다. 따라서 **중복된 데이터가 있는 경우에만 중복을 제거하고 데이터를 리패칭**하기로 한다.

#### 중복 여부에 따라 데이터 재구성

reduce 메서드를 사용하여 중복된 데이터를 삭제한 대시보드 배열을 재생성하고 길이가 6 이하일 경우엔 중복 데이터가 존재하여 제거된 상황이니 원하는 사이즈의 배열을 만들기 위해 재귀 함수를 호출하여 사이즈를 맞추도록 했다.

```js
const checkDashboard: DashboardDetail[] = dashboards.reduce(
  (acc: DashboardDetail[], cur: DashboardDetail) => {
    if (acc.findIndex(({ id }) => id === cur.id) === -1) {
      acc.push(cur);
    }
    return acc;
  },
  []
);

if (checkDashboard.length < 6) {
  setDashboardList(
    await makeDashboardArr(checkDashboard, page, 6 - checkDashboard.length)
  );
  return;
}

setDashboardList(checkDashboard);
```

중복이 제거되어 사이즈가 모자란 배열과 해당 데이터의 페이지 넘버, 데이터 사이즈를 매개변수로 받아서 사이즈만큼 데이터를 배열에 채워주는 함수를 만들었다.

```js
export default async function makeDashboardArr(arr, page, size) {
  async function reFetch() {
    // 다음 페이지의 데이터를 받아와 필요한 만큼 넣기 위해 페이지 넘버에 +1을 함
    const data = await getDashboard(page + 1);
    const { dashboards } = data;
    return dashboards;
  }

  const fetchArr: DashboardDetail[] = await reFetch();

  // 함수 내에서도 중복 제거를 이어서 진행함
  const checkDashboard: DashboardDetail[] = fetchArr.reduce(
    (acc: DashboardDetail[], cur: DashboardDetail) => {
      if (acc.findIndex(({ id }) => id === cur.id) === -1) {
        acc.push(cur);
      }
      return acc;
    },
    []
  );
  arr.push(...checkDashboard);

  // 원하는 사이즈만큼의 배열이 만들어지지 않는다면 다음 페이지로 넘어가고
  // 모자란 데이터의 갯수만큼 데이터를 받아와서 채울 수 있도록 사이즈를 조절했다.
  if (arr.length < 6) {
    makeDashboardArr(arr, page + 1, size - arr.length);
  }

  return arr;
}
```

코딩 테스트를 연습하며 알고리즘 공부를 할 때만 해도 이런 것들이 대체 어디에 적용이 될까, 내가 언제 이런 코드를 짤 수 있을까 회의감을 느끼면서 공부했던 적이 있는데 이번 문제를 해결하면서 빛을 발휘한 것 같다. 테스트를 칠 때 만드는 재귀함수는 동작 흐름도 이해가 안될 때가 많았는데 내가 직접 여러 상황을 고려하면서 만들어보니 너무 뿌듯했다. 단순히 백엔드의 결함이라고 넘어가지 않았던 과거의 나를 칭찬하고 싶다.
실제 현업에서도 이런 문제가 발생했고 만약 api가 당장 수정이 안되는 경우라면? (휴가라던가.. 출장이라던가..) 게다가 이 문제는 유저에게 중복된 데이터를 노출시켜 서비스 이용에 혼란이 올 수도 있는 심각한 버그라고 생각했다. 네트워크 비용이 좀 더 들더라도, 그리고 api 수정이 완료되어 미래에 쓰이지 않을 코드이더라도 당장에 버그라는 불을 끄는 데 성공해서, 그리고 내가 좀 더 성장한 것 같아 기분이 좋다.
