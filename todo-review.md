---
date: '2024-07-02'
title: 'Todo-Todo'
categories: ['Review']
tags: ['Project', 'Todo-Todo', 'Review']
thumbnail: 'https://github.com/user-attachments/assets/ee72ad43-3178-4e28-92d4-ddabb2e8857d'
---

<div align=center>

![시연영상](https://github.com/user-attachments/assets/9496a6ed-33ba-4e1a-a3e8-1eb0348e0498)

</div>

## Todo-Todo

Todo-Todo는 초대한 멤버들과 함께 대시보드를 생성하여 할 일을 공유하는 칸반보드 서비스이다. 평상시에도 많이 사용되는 서비스라 기능 확장이나 추후 사용에 용이할 것 같아 선택했다. 우리 팀은 이번 기회에 Next의 app router를 프로젝트에 사용하여 서버 컴포넌트와 클라이언트 컴포넌트에 익숙해지는 것을 목표로 했다.

### 기술 스택과 그 이유

- Next.js 14
  - SSR, SSG를 지원하고 리액트를 기반으로 만들어진 Next는 리액트에 익숙한 우리에게 좋은 선택지였다.
  - SSR을 위해 따로 함수를 만들지 않아도 되고 리액트 18부터 제공되는 서버 컴포넌트를 사용할 수 있기 때문에 13버전 이상을 골라 사용했다.
  - 페이지 단위가 아닌 컴포넌트 단위로 렌더링을 조절할 수 있기 때문에 사이트의 성능이 더 좋아질 수 있다.
- TypeScript
  - 런타임 이전에 타입 체크를 해서 사전에 에러를 방지할 수 있기 때문에 타입스크립트는 선택이 아닌 필수였다.
- Tailwind.css
  - 서버 컴포넌트를 스타일링하기 위해 선택한 기술이다.
  - 클래스 명으로 스타일을 지정하여 UI와 비즈니스 로직을 분리하는 데 크게 영향이 없다.

### user flow

유저 플로우는 기본 요구 사항과 피그마 디자인을 보며 랜딩페이지에서부터 유저의 이벤트에 따라 이동되는 경로를 정리했다.

![유저플로우](https://github.com/user-attachments/assets/fe806427-a8f5-44c7-84ee-0c9d0fa3003d)

### 원활한 협업을 위한

우리 팀은 우선 프로젝트에 대해 문서화 작업을 하며 동시에 R&R 분배를 시작했다. [노션 페이지](https://hazeee.notion.site/14-3aab6bcf970b4e9c86267aa299d8e3be)를 만들어 필요한 문서를 하나의 페이지에 모으고 컨벤션을 기록했다. 또한 매 스크럼마다 각자의 내용과 추가적인 의견에 대한 내용도 모두 기입했다. UX에 대한 고민을 공유하여 기존 기획안에서 보충할 수 있는 부분은 논의한 후 수정하여 구현했고, 서로 다른 의견을 주장하는 것에 대한 근거를 같이 제시할 수 있는 경험을 얻게 되었다.

![회의록](https://github.com/user-attachments/assets/4f943690-8570-4d8a-9728-2f45733a7adb)

![회의 내용](https://github.com/user-attachments/assets/1d6949ec-c19d-40fb-b9d2-fa00c60d9dab)

프로젝트 구현은 이슈를 기반으로 브랜치를 생성하여 진행, 다른 협업 툴은 사용하지 않고 깃헙에서 제공되는 이슈와 칸반보드를 활용했다. 팀 프로젝트인 만큼 맡은 기능별로 데드라인을 정해 이슈 제목에 기입하여 강제성을 살짝 주입하고 어렵거나 시간이 모자란 경우 빠른 공유를 통해 팀원에게 알리고 같이 해결할 수 있도록 했다.

![깃헙 이슈 목록](https://github.com/user-attachments/assets/0465ec26-68e1-437a-a179-c93914ffebfa)

## 맡았던 역할

- Team Leader
- 랜딩 페이지 페이지 디자인 구현
- 대시보드 설정 페이지 UI 구현
- 대시보드 정보 수정 PUT 요청 및 요청 후 데이터 패칭
- 멤버, 초대내역, 대시보드 목록 데이터 페이지네이션 구현
- 초대 내역 검색 기능 구현

### Team Leader

좋은 팀원들을 만난 덕분에 팀장으로서 프로젝트 외적으로 관리하고 신경써야 할 부분들을 많이 나눠서 진행해주셔서 너무 감사하다. 문서 관리를 위한 노션 페이지와 프로젝트 기본 세팅, 레포지토리 세팅 등 모두 팀원들 덕분에 쉽게 진행할 수 있었던 것 같다. 팀장으로서 내가 신경썼던 것은 두 가지였다.

1. 프로젝트 진행 시 의견을 편하게 제안할 수 있는 환경을 조성하자
2. 팀원들의 사기가 저하되지 않도록 팀장으로서의 역할에 집중하자

스프린트에서는 파트가 시작될 때 팀 구성이 완료된 후에 본격적인 프로젝트 돌입까지는 2주~한달의 시간이 주어진다. 그 시간동안 프로젝트에 쓰일 기술들을 공부하고 팀원들과의 유대를 쌓는다. 나는 이 시간을 활용해서 프로젝트 시작 전부터 프로젝트를 위한 기반을 하나씩 쌓기 시작했다. 처음부터 노션 문서를 만들어 팀원들이 자유롭게 의견을 공유할 수 있도록 했고, 매일 진행되는 팀미팅에서는 과제 외로 아이스 브레이킹 시간을 가져 유대감을 쌓았다. 나는 부산에 거주해서 위워크에 자주 갈 수 없기 때문에 특히 더 신경쓴 것 같다. 좋은 팀원들을 만나서 인 것도 있지만 그런 시간들 덕분에 프로젝트를 진행할 때 팀원들 모두가 사소한 것이라도 편하게 제안을 했고, 반대 혹은 찬성의 의견 또한 편하게 할 수 있게 되었다.

팀장으로서의 역할은 대단한 일을 맡은 건 아니였다. 다들 많이 도와줘서 문서 관리나 세팅 관련한 여러 부분들을 도와줄 수 있었고, 그런 팀원들에게 보답하는 것은 팀원들이 기피하는 일들을 내가 맡아서 하는 것이었다. 스프린트에서 제공되는 프로젝트 기획서의 8할은 랜딩페이지가 존재하는 프로젝트인데, 아무래도 별 다른 기능없이 디자인을 구현하는 것이라 모두가 하기 싫어하는 페이지였다. 또 재밌는 것이 나를 제외한 모든 팀원들이 이전 프로젝트에서 해당 페이지를 맡아서 했었다. 퍼블리싱을 공부한 적이 있어서 CSS는 어렵지 않았고 랜딩 페이지는 내가 가져가기로 했다. 추가로 또 싫어했던 일이 발표였다ㅎㅎ 프로젝트 마지막엔 발표하는 시간이 주어지는데, 막바지에 결혼식과 장례식이 겹쳐 시간이 부족했지만 책임지고 밤새 발표 자료와 스크립트를 만들어 준비했다. 팀원들 모두 만족해하고 발표 자료를 본 동기들이 재밌어해서 뿌듯했다.

### 랜딩 페이지, 대시보드 설정 페이지 UI 구현

Tailwind를 처음 써봐서 CSS 구현이 마냥 쉽진 않았다. Tailwind는 일반적인 CSS의 속성과는 다른 이름으로 클래스명을 사용한다. 속성과 그 값을 이름 하나에 담아야 하기 때문인데, 예를 들어 `width: 100%` 라는 속성을 사용하고 싶다면 `w-full` 이라고 써야한다. [공식 문서](https://tailwindcss.com/docs/installation)를 통해 필요한 속성을 하나씩 찾느라 시간이 조금 지체됐는데 프로젝트가 지날 수록 익숙해져서 어느 정도 시간이 지나니 검색없이 스타일링하는 경지가 되었다.

![설정 페이지](https://github.com/user-attachments/assets/11995d50-fd6a-4c1b-8cdb-dbec6410af8d)

app router의 경우 이전에 사용된 page router와는 달리 페이지 단위로 레이아웃 설정이 가능하다. 그 덕분에 대시보드 페이지 내부에서만 사용될 헤더를 보다 쉽게 설정할 수 있었다. 공통되는 페이지네이션 기능과 버튼 등은 컴포넌트로 분리했다.

### 초대 내역 검색 기능 구현

<div align=center>

![검색기능](https://github.com/user-attachments/assets/91ffec49-f53d-4a17-ae13-507d256c7021)

</div>

검색 기능은 input 창에 데이터가 입력될 때마다 api 요청을 보내 응답 데이터를 화면에 매핑해야 했다. 사전에 팀원이 인피니티 스크롤을 구현해놓았고, 검색된 데이터 또한 인피니티 스크롤로 진행되어야 했다. 나는 검색 값의 존재 여부에 따른 분기점을 생성하기 위해 container 컴포넌트를 만들어 데이터 패칭을 하게 했다.

```jsx
export default function InvitationContainer({
  initialInvitations,
  initialCursorId,
}: InvitationListProps) {
  const [invitationList, setInvitationList] =
    useState<Invitation[]>(initialInvitations);
  const [apiCursorId, setApiCursorId] = useState(initialCursorId);
  const [searchWord, setSearchWord] = useState<string>('');
  const { ref, inView } = useInView();

  async function loadMore() {
    // NOTE - 더 이상 로드할 데이터가 없는 경우
    if (apiCursorId === null) return;
    const data: InvitationResponse = await getInvitations(
      INITIAL_NUMBER_OF_USERS,
      apiCursorId ?? undefined,
      searchWord ?? undefined
    );
    const { invitations, cursorId } = data;

    // NOTE - 새 데이터를 기존 데이터에 추가하고 cursorId를 업데이트
    setInvitationList((prevInvitations) => [
      ...prevInvitations,
      ...invitations,
    ]);
    setApiCursorId(cursorId);
  }
  async function searchInvitation() {
    const data: InvitationResponse = await getInvitations(
      INITIAL_NUMBER_OF_USERS,
      undefined,
      searchWord ?? undefined
    );
    const { invitations, cursorId } = data;

    setInvitationList(invitations);
    setApiCursorId(cursorId);
  }

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setSearchWord(e.target.value);
    searchInvitation();
  };

  const debouncedOnChange = debounce<typeof onChange>(onChange, 500);

  useEffect(() => {
    searchInvitation();
  }, [searchWord]);

  useEffect(() => {
    if (inView) {
      loadMore();
    }
  }, [inView]);
  return (
    <>
      <input
        placeholder="검색"
        onChange={debouncedOnChange}
      />
      <ul>
        <InvitationList
          invitationList={invitationList}
          setInvitationList={setInvitationList}
          setApiCursorId={setApiCursorId}
        />
        <div ref={ref} />
      </ul>
    </>
  );
}
```

추가로 `debounce` 함수를 생성하여 검색 값이 일정 시간동안 기입되지 않을 경우에 데이터 요청이 일어나도록 조절하여 네트워크 비용을 낮췄다.

```js
export const debounce = <T extends (...args: any[]) => any>(
  fn: T,
  delay: number
) => {
  let timeout: ReturnType<typeof setTimeout>;

  return (...args: Parameters<T>): ReturnType<T> => {
    let result: any;
    if (timeout) clearTimeout(timeout);
    timeout = setTimeout(() => {
      result = fn(...args);
    }, delay);
    return result;
  };
};
```

#### 프로젝트를 끝내며..

기본 요구사항에 대한 구현은 일주일안에 끝내는 걸로 잡아서 빠르게 진행되었지만 테스트 기간을 지정하지 않아 프로젝트 막바지에 급하게 버그를 잡고 리팩터링하는 시간을 가졌다. 분명 여유로울 거라 생각했던 프로젝트는 마지막까지 밤을 새게 만들었다. 또한 시간적 여유가 없어 fetch 로직에 대한 에러 처리나 중복되는 로직을 정리하지 못하는 일도 발생했는데 아무래도 코드의 완성도나 아키텍처에 대한 욕심이 많아서 기본적인 구현이 늦어지는 경우가 자주 발생하여 그런 것 같다. 프로젝트를 진행할 때는 기능 구현을 최우선으로 생각하여 우선 동작하는 코드가 되도록 해야할 것 같다. 추가로 중간에 프로젝트를 점검하는 날을 설정한다면 미리 리팩토링하고 버그를 잡을 수 있는 기회가 생겨 막바지에 급해지는 일은 줄일 수 있을 것 같다. 또한 적당한 주기를 두고 적용한 기술들을 정리하거나 회고를 진행하여 미래에 똑같은 실수를 하지 않도록 계속해서 기록해야겠다.
