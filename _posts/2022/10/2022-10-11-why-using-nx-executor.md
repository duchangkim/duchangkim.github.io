---
layout: post
title: Nx에서 Executor를 사용해야 하는 이유
category: monorepo
created_date: 2022-10-11
modified_date: 2022-10-11
author: duchangkim
tags: [monorepo, nx]
search_keyword: [monorepo, nx]
---
***

Executor는 코드에서 작업을 수행합니다. 이는 building, linting, testing, serving 등 여러 작업이 포함될 수 있습니다.

Executor와 shell script / npm script 사이에는 두가지 주요 차이점이 있습니다.

Executor는 프로젝트에서 유사한 작업을 수행하기 위한 방법을 일관성있게 만들 수 있도록 도와줍니다. → `nx build project1`이 project1을 빌드한 것 처럼 project2의 설정파일을 살펴보지 않아도 `nx build project2`가 기본 설정으로 project2를 빌드할 것이라고 예측할 수 있게 합니다.

Nx는 이러한 일관성을 활용하여 여러 프로젝트에서 동일한 Executor를 실행할 수 있습니다. 
→ `nx affected --target=test` 으로 현재 코드 변경의 영향을 받는 모든 프로젝트에서 테스트를 실행합니다.

# 결론
프로젝트의 일관성을 위해 각 library project.json에 executor를 정의하여 사용해야 할 것 같습니다.

# 참고 문서
[use-task-executors](https://nx.dev/plugin-features/use-task-executors)