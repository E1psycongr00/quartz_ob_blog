---
tags:
  - 완성
  - Husky
  - JS
  - Commit
aliases: 
title: Husky를 활용해 Commit Convention 강제하기
date: 2024-02-22
---
작성 날짜: 2024-02-22
작성 시간: 13:10


----

## 문제 & 원인
### Commit 컨벤션을 지키지 못해 실수 할 때가 있다

#### 사람은 누구나 실수한다

커밋 컨벤션을 따를 때 body와 header 사이에 빈칸을 쓰지 않는다던가 실수로 feat을 faet 로 쓴다던가, refactor 를 reafctor로 쓰는 등, 잘못 쓰고 push를 하면 이런 걸로 코드 리뷰하기도 좀 그렇고, 뭔가 보기 아쉬울 때가 많다.


>[!info] 커밋 종류
>- [udacity commit convention](https://udacity.github.io/git-styleguide/)
>- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

#### 원격 저장소에 push되면 되돌리기 쉽지 않음

개발을 하다 보면 개발에 쫒기거나 피곤하면 commit 실수하고 push하는 경우가 있다. 강제로 되돌리는 방법이 있지만 원격 저장소에서 강제로 커밋을 제어하는 것은 항상 두려운 일이다. 


## 해결 방안
### 해결 전략
Git을 사용한다면 Commit 을 컨벤션에 따라 강제해서 commit을 막을 수 있는 방법이 있다. 바로 GitHook을 이용하는 것이다. GitHook은 이름 그대로 Git의 commit, push 같은 이벤트에 대해서 Validation에 따라 동작 여부를 결정할 수 있게 해준다.

### Hursky와 Commitlint
[[Husky]]을 활용하면 Githook을 매우 쉽게 활용할 수 있다. 그리고 Commit convention은 Commitlint 패키지를 설치하면 쉽게 사용할 수 있다.

```
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

위와 같이 설치하고 루트 폴더에 commitlint.config.js를 만든다.

```js
module.exports = {
    extends: ["@commitlint/config-conventional"],
};
```

위는 미리 구현된 컨벤션으로  [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)를 따른다. 구체적인 구현과 convention 적용 방식은 [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)을 참고 하자.

.husky 폴더 내부에 commit-msg 파일을 생성하고 다음과 같이 입력한다
```
// path: .hursky/commit-msg
npx --no-install commitlint --edit --strict
```

>[!caution] commitlint 사용시 주의
>rule이 error가 아닌 warning인 경우 commit을 블락하지 않고 그대로 실행하는 경우가 있다. 따라서 엄격하게 컨벤션을 적용하고 싶다면 --strict를 꼭 붙여주자
## 질문 & 확장

(없음)

## 출처(링크)
- https://udacity.github.io/git-styleguide/
- https://github.com/conventional-changelog/commitlint
## 연결 노트
- [[Husky]]