# Git 실무 명령어 - add/commit/push 그 다음
기초는 알지만 실수 수습할 줄 모르면 반쪽짜리다. | 2026-04-10

### 개념
실무에서 Git을 쓰다 보면 잘못된 커밋을 되돌리거나, 브랜치를 합치거나, 특정 커밋만 골라 가져오는 상황이 생긴다.
add/commit/push만 알면 사고가 났을 때 손을 못 쓴다. 자주 쓰는 수습 명렁어를 익혀두자.

## 실수 수습 명렁어
### 커밋 되돌리기
`git revert HEAD` 마지막 커밋을 되돌리는 새 커밋 생성. History 보존 → 팀 작업에 안전.
`git reset --soft HEAD~1` 마지막 커밋 취소, 변경사항은 staged 상태로 유지. 커밋 메시지 수정할 때.
`git reset --hard HEAD~1` 마지막 커밋 + 변경사항 모두 삭제. 되돌릴 수 없으니 신중하게..!

### 브랜치 합치기
`git merge feature` feature 브랜치를 현재 브랜치에 합침. merge commit 남음.
`git rebase main` 현재 브랜치의 커밋을 main 위에 재배치. 히스토리가 깔끔해짐.

### 유용한 도구
`git stash` 작업 중인 변경사항을 임시 저장. 급하게 브랜치 바꿔야 할 때.
`git stash pop` stash에 저장해둔 변경사항 다시 꺼내기.
`git cherry-pick abc1234` 특정 커밋 하나만 골라서 현재 브랜치에 적용. (abc1234는 해당 커밋의 ID값)
`git log --oneline --graph` 브랜치 히스토리를 그래프로 보기 좋게 출력.

## REVERT vs RESET 핵심 차이
```
revert - 히스토리 유지. 협업 시 안전. git revert HEAD → "되돌렸다"는 커밋이 남음
reset --hard - 히스토리 삭제, 혼자 쓸 때만. git reset --hard HEAD~1 → push된 커밋엔 절대 쓰지 말 것
```

> **stash는 임시 서랍** 브랜치를 급히 바꿔야 하는데 커밋하기 애매할 때, git stash로 잠깐 넣어두고 나중에 git stash pop으로 꺼내면 된다.

> **push된 커밋에 reset --hard 금지** 이미 원격에 올라간 커밋을 강제로 되돌리면 팀원의 히스토리가 꼬인다. push된 건 항상 revert로.
