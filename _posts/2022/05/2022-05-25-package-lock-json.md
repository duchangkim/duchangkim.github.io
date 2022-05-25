---
layout: post
title: package.lock.json
category: npm
created_date: 2022-05-25
modified_date: 2022-05-25
author: duchangkim
tags: [npm]
search_keyword: [npm]
---
***

회사에서 "lockfile은 왜 github에 올려요?" 라는 질문에 "남들이 올려서 그냥 올리는건데요??" 라고 답변은 했지만, 뭔가 이유가 있지 않을까 해서 찾아보게 되었습니다.

## package.lock.json파일이란?
`package.lock.json`은 npm이 `node_modules` 트리 또는 `pacakge.json`을 수정하는 모든 작업에 대해서 자동으로 생성되는 파일입니다. 종속성 업데이트에 관계없이 후속 설치에서 동일한 `node_modules` 트리를 생성할 수 있도록 정확한 트리를 설명합니다.

`package.lock.json`은 소스 저장소에 커밋하기 위한 것이며 다양한 용도로 사용됩니다.

## package.lock.json의 역할
- 팀원, CI/CD에서 정확히 동일한 종속성을 설치하도록 보장하는 종속성 트리를 설명합니다.
    - `package.json`의 (dev)dependencies 버전에 틸드(~), 캐럿(^) 를 사용하지 않고 작성한다 해도 그 의존성(패키지)이 내부적으로 의존하고 있는 버전까지 고정할 수 없습니다.
- 프로젝트 자체(`node_modulse`까지)를 커밋하지 않고도 `node_modules`를 이전 상태로 되돌릴 수 있는 
”시간 여행” 기능을 제공합니다.
- 소스 제어(git) diff를 통해 트리 변경사항에 대한 내용을 쉽게 확인할 수 있습니다.
- 이전에 설치된 패키지에 대해 반복되는 metadata확인을 건너뛸 수 있도록 하여 설치 프로세스를 최적화 합니다.
- npm v7 이상부터는 패키지 트리에 대한 완전한 그림을 얻을 수 있는 충분한 정보가 포함되어 있어 `package.json` 파일을 읽을 필요성이 줄어들고, 성능이 크게 향상됩니다.

[package.lock.json description](https://docs.npmjs.com/cli/v8/configuring-npm/package-lock-json#description)