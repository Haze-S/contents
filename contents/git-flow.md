---
date: "2024-03-21"
title: "[Git] branch 전략"
categories: ["Git", "WIL"]
summary: "Git-flow vs Github-flow"
thumbnail: "https://github.com/Haze-S/blog-contents/assets/87344625/807ab80a-96c4-481a-866e-8cbc81ce0e6b"
---

# Git flow

![git-flow1](https://github.com/Haze-S/blog-contents/assets/87344625/807ab80a-96c4-481a-866e-8cbc81ce0e6b)

git flow란 Vincent가 2010년에 제시한 [Git 브랜치 전략](https://nvie.com/posts/a-successful-git-branching-model/)으로 여러 사람들이 한 저장소에서 작업을 할 때, 즉 협업 시에 필요한 전략이다. branch의 생성과 삭제, 병합 등 각 행동에 있어서 규칙을 설립하고 각 branch에 의무를 부여하여 개발자들의 혼란을 최대한 줄인다.

당연히 이런 약속들은 팀 내에서 서로 조율을 통해 정해도 되지만 git flow와 같이 긍정적인 효과로 많은 사람들이 사용하고 있는 전략을 사용하면 팀원 모두의 이해와 중간에 투입되는 인원의 이해도 쉬울 것이다. 이번 글에선 git flow가 어떤 전략으로 구성되어 있는 지 정리해본다.

## Git flow branch

git flow에는 다섯 종류의 브랜치가 존재한다.

- **항상 유지되는 메인 브랜치**
  - master(main)
  - develop
- **merge 후 삭제 되는 보조 브랜치**
  - feature
  - release
  - hotfix

### master(main)

라이브 서버에 제품으로 출시되는 브랜치로 배포 가능한 상태만 관리하는 곳이다. 절대 master 브랜치에서 작업하지 않으며 tag를 이용하여 버전을 기록한다. _ex) 1.n : 기능 추가_

### develop

_**master로 부터 분기된 브랜치**_ 로 다음 버전을 출시하기 전 모든 개발은 이 곳에서 이루어진다. 실제 서비스가 배포되는 구간과 **_배포 전 개발을 진행하는 구간_** 의 중간 다리 역할이다. develop 브랜치는 프로젝트의 모든 기록을 가지고 있으며 수시로 버그 수정을 위한 커밋들이 추가된다. rebase를 통해 커밋 이력을 깔끔하게 정리해서 어느 시점에 반영을 했는지 추적하기도 한다.

### feature

서비스의 추가 기능을 개발하는 브랜치다. 절대 master 브랜치와 바로 연결되어선 안되며 **_develop에서 분기되어 다시 develop에 병합_** 되어야 한다. 이력을 깔끔하게 보려면 develop에서 분기된 시점을 잘 관리해야하여 새로운 feature 브랜치는 가장 **_마지막에 반영 된 develop 브랜치에서 생성_** 되어야 한다.

### release

서비스의 다음 버전을 최종적으로 배포하기 전 마지막 점검 구간이다. develop에서 다음 버전을 위한 개발이 모두 완료되었다면 **_QA를 위해 develop으로 부터 생성_** 하며 에러 수정과 문서 작성 그리고 다른 출시 관련 작업들 모두 해당 브랜치에서 작업한다. **출시 준비가 완료되었다면, master 에 버전을 태깅하여 merge**하고, release 단계에서 발견된 수정 사항 또한 develop에 반영되야 하기 때문에 **develop에도 merge**한다.

### hotfix

이미 배포된 버전에서 유지보수나 버그로 인해 빠르게 수정해야 할 부분이 있을 때 master 에서 분기되는 브랜치이다. 버그에 대한 수정이 완료된 후에는 develop과 master에 반영하며 태깅을 통해 정보를 기록한다. 버그를 잡는 동안에도 다른 사람들은 develop에서 하던 일을 계속 할 수 있다. release가 진행되고 있는 상태라면 release에도 반영이 되도록 해야하며 버그를 고친 후에는 제거되는 일회성 브랜치이다.

## Git flow 순서

git flow의 규칙에 따라 개발 순서를 적용하자면 아래와 같다.

1. 서비스의 소스 코드를 관리할 저장소를 만든다.(master)
2. 저장소에 develop 브랜치를 생성하여 default branch로 설정한다.
3. 개개인의 작업이 끝나면 develop 브랜치에 병합한다.
4. develop에서 모든 개발이 끝나면 release로 분기한다.
5. release에서 문제가 없으면 master와 develop에 병합한다.
6. master에서 버전 태그와 함께 배포한다.
7. 배포된 버전에 버그가 생겨 수정이 필요한 경우 hotfix를 만들어 작업한다.
8. hotfix에서 수정이 끝나면 master와 develop에 병합한다.

# Github flow

![git-flow2](https://github.com/Haze-S/blog-contents/assets/87344625/1eb43a91-ac9e-4892-838b-d5ea2e94d631)

git flow에 관해 구글링하며 정리하던 중 [_github flow_](https://docs.github.com/en/get-started/using-github/github-flow)를 알게 되었다.

github flow는 git flow와는 다르게 규칙과 흐름이 단순하며, Github의 창립자인 Scott Chacon이 git flow는 github에 적용하기 복잡하다는 이유로 만들어졌다. git flow를 제시했던 Vincent도 git flow는 버전 관리가 필요하고 롤백이 잦은 상황이 많은 모바일 앱에서 적합한 flow라고 게시한 적이 있다.

일반적인 웹 앱은 롤백이 되거나 버전이 배포되는 기간이 있기 보단 항상 최신 버전으로 유지되며, 롤백보단 버그 수정이 더 많이 이루어 진다. Vincent도 웹 앱에선 github flow를 대안으로 제시했다.

## Github flow branch

github flow에는 2가지의 브랜치만 존재한다. main(master)에만 대한 규칙을 엄격히 규정하고 그 외 브랜치에는 별다른 규칙을 정하지 않는다. 따라서 main을 제외한 다른 브랜치들은 생성과 삭제, 의무들이 자유롭다. 다만 merge가 필요할 땐 Pull Request 기능을 사용하도록 권장한다.

### Github flow 전략

<p align=center>
  <img src="https://github.com/Haze-S/blog-contents/assets/87344625/3fdd0ca1-5196-4fbe-896f-539bd8df3bb6" />

[이미지 출처](https://subicura.com/git/guide/github-flow.html)

</p>

- main 브랜치는 항상 최신 상태이며 저장소에 항상 존재하고 서비스에 배포되는 브랜치이다.
  - 따라서 main 브랜치에 merge할 때는 엄격한 룰과 함께 테스트를 거쳐야 한다.
- 새로운 브랜치는 항상 main에서 분기된다.
  - main에서 분기되어 생성되는 브랜치는 어떤 이유로든 자유롭게 생성이 된다.
  - 다만 체계적인 분류 없이 브랜치가 만들어 지는 것이기 때문에 이름을 통해 해당 브랜치가 하는 일을 명확하게 나타내는 것이 중요하다.
- 커밋 메세지를 명확히 남겨 원격 저장소에 수시로 push 한다.
  - 원격 저장소에 자신이 하는 일을 다른 사람들도 확인할 수 있도록 한다.
  - PR을 생성하여 팀원들과 코드 리뷰를 하고 merge 준비를 마친 후 main에 반영한다.
    - main에 반영 된다면 라이브 서버에 배포되는 것이니 충분한 리뷰를 진행해야 한다.
- 테스트 환경에도 문제가 발생되지 않는다면 main으로 merge된다.
  - main 브랜치가 가장 최신 브랜치이기 때문에 자동화 도구를 이용하여 즉시 배포한다.
  - Github flow의 핵심 장점인 CI/CD가 이루어진다.

## Git flow vs Github flow

개인적으로 github flow는 팀 내에서 코드 리뷰와 커뮤니케이션이 원활이 이루어지지 않는다면 위험하거나 급하게 버그를 수정해야할 상황이 생길 수도 있겠다는 생각이 들었다. 큰 그림은 github flow 처럼 하되, release 브랜치는 버리고 develop 브랜치는 취합하여 합쳐서 진행하는 것도 좋을 것 같다. 두 전략 모두 장단점이 있겠지만 협업을 하는 상황이라면 Git flow가 보다 더 안정적일 것 같다는 생각이 든다. 그래도 각 전략에 맞는 상황들을 혼자 생각해 정리해보자면 아래와 같을 것 같다.

```markdown
- Git flow
  - 명확한 버전 관리와 릴리즈 기간이 필요한 모바일 서비스 환경
  - 많은 인원이 참여하여 정확하고 명확한 규칙 아래 개발이 진행되어야 하는 환경
  - 따라서 주기적으로 배포, QA, hotfix 등을 수행할 인원이 있는 팀
- Github flow
  - 수시로 릴리즈 되는 서비스를 지속적으로 배포하는 서비스 환경
  - 비교적 규모가 작거나 크더라도 커뮤니케이션이 활발이 이루어지는 팀
```
