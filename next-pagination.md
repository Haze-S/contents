---
date: '2024-06-30'
title: 'app router의 페이지네이션'
categories: ['Note']
tags: ['Project', 'Todo-Todo', 'Next14']
thumbnail: 'https://github.com/user-attachments/assets/5624702f-79cd-44de-b6fb-a0d8fe9f0390'
---

## 페이지네이션 공통 컴포넌트화

이번 프로젝트에서 페이지네이션 기능은 총 4군데에서 사용이 가능했다. 따라서 우리팀은 해당 기능을 공통 컴포넌트로 빼기로 했고, 이 과정에서 겪은 일들을 기록하고자 한다.

### 댕같은 컴포넌트 분업화

제일 먼저 마주한 문제는 이번 프로젝트가 Next 14버전을 사용했고, 우리는 app router를 사용하고 서버 컴포넌트와 클라이언트 컴포넌트가 하는 일이 달랐다는 것이다. Next의 app router에서는 react 18 버전과 함께 사용되어 컴포넌트 단에서 서버 렌더링과 클라이언트 렌더링이 가능한 것이였는데 이런 점들이 이전에 리액트나 page router로 구현했을 때와는 상황이 달라져 많이 해멨던 것 같다.
페이지네이션 기능만을 봤을 때, 기본적인 요구 사항은 아래와 같았다.

- 페이지를 이동하는 버튼은 전과 후만 있으며 따로 페이지 넘버 링크를 표기하지 않는다.
- 페이지가 이동될 때마다 해당 페이지 번호를 서버로 넘겨 원하는 사이즈만큼 데이터를 받아온다.
- 따라서 페이지 번호는 클라이언트에서 관리하고 그에 따른 데이터 패치가 이루어져야한다.

app router에서 사용되는 서버 컴포넌트와 클라이언트 컴포넌트는 각자 할 수 있는 일이 달랐다. 간단하게 생각하면 어떠한 기능이 필요할 때 기본적으로 _'클라이언트가 없어도 실행 가능한가?'_ 는 서버 컴포넌트이고, _'클라이언트가 있어야 실행가능한가?'_ 는 클라이언트 컴포넌트에서 이루어졌다.

<div align=center>

![공식문서 컴포넌트](https://github.com/user-attachments/assets/73d820fd-fe47-4e86-96f7-f4fc18c0b52a)

</div>

여기서 살짝 멘붕이 왔다.

이전에 리액트에서 페이지네이션 기능을 만든다면 공통 컴포넌트로 페이지 총 갯수와 페이지 번호를 props로 넘겨 페이지 이동을 발생시키고, 해당 컴포넌트를 사용하는 쪽에서는 페이지 번호를 상태로 관리하여 데이터 페칭하는 함수와 함께 데이터를 불러왔다.
하지만 컴포넌트가 할 수 있는 일이 달라지면서 하나의 컴포넌트에서 데이터 패칭과 react 훅을 함께 쓸 수 없게 되었다. 여자저차 분리하게 되더라도 페이지 넘버는 어떻게 공유할 것인지가 문제였다.

### 친절한 next 문서

이를 해결하기 위해 next의 페이지네이션에 대해 알아봤고 Next의 공식 문서에서는 아주 친절한 튜토리얼도 함께 기재해주었다. [공식 문서](https://nextjs.org/learn/dashboard-app/adding-search-and-pagination)

![next 공식문서](https://github.com/user-attachments/assets/5624702f-79cd-44de-b6fb-a0d8fe9f0390)

Todo 프로젝트에서 페이지네이션 기능은 하나의 페이지에 2개의 페이지네이션이 있고, 모든 페이지에서 사용되는 사이드바에도 페이지네이션 기능이 사용됐다. 처음에는 문서에 있는 튜토리얼과는 유스케이스가 다르다 생각했으나 좀 더 고민해보니 page 번호를 컴포넌트끼리 공유할 때 쿼리를 사용하면 된다는 아이디어를 얻게 되었다.

#### pagination 공통 컴포넌트

```jsx
// 한 페이지에 2개 이상의 페이지 번호가 필요한 경우가 있기 때문에
// 쿼리 키를 props로 받아서 따로 관리할 수 있게 구현했다.
export default function Pagination({
  paramKey,
  currentPage,
  lastPage,
}: PageButtonProps) {
  const searchParams = useSearchParams();
  const { replace } = useRouter();
  const pathname = usePathname();
  // state 대신 쿼리 스트링을 이용하여 page를 관리한다.

  const goToForwardHandler = () => {
    const params = new URLSearchParams(searchParams);
    params.set(paramKey, String(Math.max(currentPage - 1, 1)));
    replace(`${pathname}?${params.toString()}`);
  };

  const goToNextHandler = () => {
    const params = new URLSearchParams(searchParams);
    params.set(
      paramKey,
      String(currentPage !== lastPage ? currentPage + 1 : currentPage)
    );
    replace(`${pathname}?${params.toString()}`);
  };

  return (
    <>
      <button
        onClick={goToForwardHandler}
        disabled={currentPage === 1}
      >
        <Image
          src={getForwardArrowSrc(currentPage, theme!)}
          alt="왼쪽을 향하는 꺽쇠 화살표"
        />
      </button>
      <button
        onClick={goToNextHandler}
        disabled={lastPage === currentPage}
      >
        <Image
          src={getNextArrowSrc(currentPage, lastPage, theme!)}
          alt="오른쪽을 향하는 꺽쇠 화살표"
        />
      </button>
    </>
  );
}
```

아래 url의 쿼리값을 보면 페이지가 이동될 때마다 쿼리값이 변경되면서 데이터가 바뀌는 것을 확인할 수 있다.

<div align=center>

![쿼리를 통한 페이지 데이터 공유](https://github.com/user-attachments/assets/2b4d6a4b-71c5-4d20-95cc-415b07d6ad8b)

</div>

사용하는 쪽에서는 쿼리 스트링 값을 사용하여 데이터를 패칭할 수 있다.

```jsx
export default function DashboardList({
  initialData,
  lastPage,
}: DashboardListProps) {
  const [dashboardList, setDashboardList] =
    useState<DashboardDetail[]>(initialData);
  const searchParams = useSearchParams();

  const paramKey = 'boardPage';
  const currentPage = Number(searchParams.get(paramKey)) || 1;

  async function getData() {
    const data = await getDashboard(currentPage);
  }

  useEffect(() => {
    getData();
  }, [currentPage]);

  return (
    <Pagination
      paramKey={paramKey}
      currentPage={currentPage}
      lastPage={lastPage}
    />
  )
}
```

이번 기능을 만들면서 서비스의 기반이되는 환경을 잘 활용하는 연습도 해야 하는 것을 느꼈다. 웹 서비스를 개발하는 만큼 웹에서 기본적인 것들은 응용하면 보다 쉽게 기능을 개발할 수 있을 것 같다.
