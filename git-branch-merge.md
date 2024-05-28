---
date: '2024-03-21'
title: '[Git] branch merge'
category: 'Note'
tags: ['Git', 'WIL']
summary: 'merge에도 다양한 개념과 방법들이 있는 걸 알게 되어서 정리해본다.'
thumbnail: 'https://github.com/Haze-S/blog-contents/assets/87344625/d90539cc-4818-4d10-8f4f-2af6aff16d09'
---

![merge1](https://github.com/Haze-S/blog-contents/assets/87344625/d90539cc-4818-4d10-8f4f-2af6aff16d09)
git은 버전 관리 시스템의 하나로서 소프트웨어 개발 중에 소스 코드를 공동으로 개발하는 프로그래머 간의 작업을 조정하는 데 사용된다. **공동으로 개발하는 작업**을 조정하기 위해 쓰이는 것이 branch와 merge인데 branch는 하나의 작업본, merge는 작업본인 branch를 기존 코드에 ‘병합’하는 것을 의미한다.

merge를 단순히 코드를 합치는 하나의 행동으로 알고 있었는데 이번 공부를 통해 merge에도 다양한 개념과 방법들이 있는 걸 알게 되어서 정리해본다.

## Merge 종류

### Fast-forward

![merge2](https://github.com/Haze-S/blog-contents/assets/87344625/12a376fb-9382-4fed-a049-fc3e52db34fe)

코드의 베이스가 되는 `Main` 에서 `branch` 가 분기된 후 `Main` 에 새로운 작업이 생성되거나 반영되지 않은 상태라면 `branch` 의 내용이 제일 최신상태가 된다. 이 땐 **HEAD의 이동만으로 merge 처리**가 될 수 있다.

Fast-forward 상태이더라도 commit 이력에 merge 내용을 남기고 싶다면 아래 두 가지 명령어를 통해 이력을 남길 수 있다.

```bash
# commit 메세지를 남기고 싶을 때 (Rebase and merge)
git merge --no-ff {브랜치명}
# fast-forward가 아니라면 merge를 막고 싶을 때
git merge --ff-only {브랜치명}
```

### 3-way-merge

![merge3](https://github.com/Haze-S/blog-contents/assets/87344625/02fa04ae-8075-49b0-91c7-d1aec81d05ac)

협업 대부분의 경우 위와 같이 3-way-merge 방식으로 이루어 진다. `branch A` 가 작업을 할 동안 다른 branch에서 main과 merge가 이루어져 main의 내용이 변경되고 후에 A 또한 merge가 이루어진다. 아래에 표시한 3가지 commit 을 고려해서 merge를 하기 때문에 3-way-merge라 칭한다.

1. 분기가 나뉘기 전 공통 조상 commit
2. 한 브랜치가 가리키는 commit
3. 다른 브랜치가 가리키는 commit

공통 조상(1번)으로 부터 **변화가 발생한 것을 우선 채택하여 merge를 실행**하며 만일 두 브랜치에 **변화가 동시에 일어난 경우 Conflict(충돌)를 발생**시켜 개발자가 해결 후에 merge 될 수 있도록 수정을 요한다.

## Merge 전략

### Merge commit

일반적으로 사용하는 merge 방법이며 해당 방법의 경우 각각의 branch에 남겨진 commit을 내역에 모두 담는다. 따라서 어떤 하나의 branch가 어디서 분기가 되었고 어떤 commit을 남겨온 뒤에 merge가 되었구나를 상세하게 파악할 수 있다.

![merge4](https://github.com/Haze-S/blog-contents/assets/87344625/2a42e17d-6c21-4fda-8e90-190faa8afb32)

하지만 이런 경우 commit 이력이 상당히 복잡해져 후에 이력을 둘러봐야 할 경우가 생기면 시간이 많이 소모될 수 있다. merge commit 명령어는 아래와 같다.

```bash
git merge {브랜치명}
```

### Squash & Merge

한 branch에 여러 commit 내용을 하나로 합친 후 merge 하는 방법이다.

![merge5](https://github.com/Haze-S/blog-contents/assets/87344625/1d8fa89a-7997-48f2-8f91-14a747903a72)

기존 commit의 내용보다 merge가 되었다는 사실에 더 초점을 둔 전략이다. 따라서 branch에서 가지고 있던 정보들이 없어지기 때문에 이력이 필요한 경우라면 사용하지 않는 것이 좋다. Squash를 통해 새로 생성된 commit(A+B+C)은 하나의 부모 commit 만 가지게 된다. 명령어는 아래와 같다.

```bash
git merge --squash {브랜치명}
git commit -m {커밋메세지}
```

Squash 는 branch의 여러 commit들을 하나로 합쳐서 깔끔하기 만들기 위해 사용하는 것이 목적이다.

### Rebase & Merge

rebase 는 말 그대로 branch가 분기된 시점인 base를 새로 설정하는 것이다.

![merge6](https://github.com/Haze-S/blog-contents/assets/87344625/a327acf7-3c0d-4abc-b7ea-3169e03ea26f)

일반적인 merge의 경우 branch마다 commit 이력이 중복으로 쌓여 복잡해지기 쉬운데 rebase를 잘 활용하면 commit history가 보다 더 깔끔하게 관리되고 충돌이 발생될 확률도 줄어든다. 아래의 세 가지 경우에 사용할 수 있을 것 같다.

- 다른 branch의 내용을 내 branch로 가져올때, commit 내역을 merge없이 깔끔하게 보이고 싶을 때
- 내 branch를 작업하는 동안 선행 branch가 변경되고 내 branch를 merge하려 했으나 충돌이 발생할때
  - 내 branch에서 선행 branch를 rebase한 다음 선행 branch에 merge
- 내 branch에서 작업한 내용을 보며 commit끼리 squash(병합)하거나, drop하고 싶을때

명령어는 아래와 같다.

```bash
git rebase {브랜치명}
```

## Don’t use rebase

rebase를 쓰는 것을 지양하는 경우도 있다.

![merge7](https://github.com/Haze-S/blog-contents/assets/87344625/469ced5b-9d97-4a25-98a6-a3056e286c6b)

위와 같이 많은 후속 branch 들을 가지고 있는 base branch의 경우이다. 모든 branch 들이 base의 최신 내용을 가지지 않은 채로 분기되어 있는 상황이고, 분기된 후속 branch의 내용이 필요하여 rebase merge를 하는 경우 해당 branch의 분기점부터 생성된 commit이 붙게 된다. 만약 분기점 이후에 생성된 commit에 또 다른 branch가 분기되었다면 그 branch의 base는 어디로 가게 될까.

![merge8](https://github.com/Haze-S/blog-contents/assets/87344625/1067e004-745c-45ab-9ad0-6d60fb0cbaf7)

후속 branch의 분기점 이후에 base에서 생성된 commit들은 없어지고 중간에 후속 branch의 내용이 들어오면서 끝에 해당 내용들이 생성되기 때문에 추후에 다른 후속 branch를 병합하게 되면 무수히 많은 conflict를 발생시킬 수 있다. 따라서 이런 경우에는 rebase를 피하는게 좋다.
