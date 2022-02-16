---
layout: post
title: 원격 저장소에 push한 commit 되돌리는 방법
category: git
created_date: 2022-02-16
modified_date: 2022-02-16
author: duchangkim
tags: [git]
search_keyword: [git, reset]
---
***

# 원격 저장소에 올라간 커밋 되돌리기

로컬에서만 작업한 커밋은 `git reset [--options] [<commit>]` 명령어로 되돌릴 수 있습니다.  
`git reset e7cecf1` -> e7cecf1 커밋으로 돌아간다는 뜻 입니다.  
또는 `HEAD` 키워드로 바로 이전 커밋으로 돌아가거나, 이전 n개의 커밋으로 돌아갈 수 있습니다.   

```shell
# 바로 이전 commit으로 돌아감
git reset HEAD^

# 이전 n개의 커밋으로 돌아감
git reset HEAD~n
```

변경사항 중 일부는 남기고 일부는 삭제해야 할 경우에는 `--soft` 옵션을 주면 됩니다.  
반대로 변경사항을 모두 폐기하고자 한다면 `--hard` 옵션을 주면 됩니다.

# 원격 저장소에 올라간 작업(커밋) 중 특정 작업만 되돌려야 하는 경우

A.js와 B.js를 수정하고 배포하였는데(원격 저장소에 올라간 상태), A.js에 문제가 있어서 당장 A.js만 되돌려야 하는 상황(B.js에는 문제가 없음)이라고 해봅시다.  

```shell
# 바로 이전 commit으로 돌아갑니다
git reset HEAD^

# 리모트 저장소에 강제로 push 합니다
git push -f origin branch

## B.js만 add, commit합니다
git add B.js
git commit -m "fix"

# 다시 리모트 저장소에 push 합니다
git push origin branch
```
위 과정을 마치면 A.js만 이전으로 돌린 상태가 됩니다.
<br>
<br>

### 참고
[https://computer-science-student.tistory.com/294](https://computer-science-student.tistory.com/294)
[git-scm.com](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)