# [2주차 - Day1] 240304 정리

### 1️⃣ Git 브랜치

- <span style="background-color:#FFFFF0"> **브랜치 생성** </span> : git branch
- <span style="background-color:#FFFFF0"> **브랜치 전환** </span> : git checkout [브랜치명]
- <span style="background-color:#FFFFF0"> **브랜치 삭제** </span>: git branch -d [삭제할 브랜치명]
- <span style="background-color:#FFFFF0"> **브랜치 생성 후 복사** </span> : git push [원격저장소별칭] [브랜치명]

### 2️⃣ 깃 브랜치 전략

- <span style="background-color:#FFFFF0"> **fast-forward 전략** </span>: 현재 브랜치의 HEAD를 대상 브랜치의 HEAD까지로 옮기는 것
  - 중간에 변경이 없을 때만 동작
- <span style="background-color:#FFFFF0"> **3-way 전략** </span> : 공통 조상 커밋과 두 브랜치의 마지막 커밋, 총 3개의 커밋을 비교하여 병합을 수행하는 방식
