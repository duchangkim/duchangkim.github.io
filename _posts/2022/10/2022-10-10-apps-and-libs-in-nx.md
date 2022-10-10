---
layout: post
title: Nx에서 apps와 libs로 패키지를 구분하는 이유
category: monorepo
created_date: 2022-10-10
modified_date: 2022-10-10
author: duchangkim
tags: [monorepo, nx]
search_keyword: [monorepo, nx]
---
***

# Application과 Library
Nx workspace(모노레포)는 일반적으로 ‘apps’와 ‘libs’로 구성됩니다. 이러한 구분은 관심사의 분리 원칙을 따르고 코드를 더 작고 응집력이 높은 단위로 구성할 수 있도록 장려합니다. 따라서 보다 모듈화된 아키텍처를 가질 수 있습니다.

Nx는 다른 ‘apps’나 ‘libs’에서 쉽게 사용할 수 있도록 tsconfig.base.json 파일에 TypeScript 경로 매핑을 자동으로 생성합니다.

```ts
// example of importing from another workspace library
import { Button } from '@my-repo/ui';
```
따라서 라이브러리를 사용하는 것은 매우 간단하며 모노레포가 아니였던 프로젝트에서 사용하는 것 처럼 익숙합니다.

하지만 전용 라이브러리 프로젝트를 갖는 것은 코드를 폴더로 분리하는 것보다 훨씬 더 강력합니다. (자바처럼 폴더 — 패키지에 접근 제한자를 사용할 수 없기 때문) 각 Nx 라이브러리에는 index.ts 배럴 파일로 표시되는 소위 ‘public API’가 있습니다. 이것은 개발자들이 무엇을 노출해야 다른 사람이 사용할 수 있고, 무엇을 노출하지 말아야 하는지에 대한 ‘API 사고’를 강요합니다.

```
Library !== published artefact
```
[라이브러리에 대한 일반적인 오해와 관련된 글](https://nx.dev/more-concepts/applications-and-libraries#misconception) 입니다.

***

application은 배포를 위해 라이브러리에 구현된 기능을 연결, 번들 및 컴파일 하는 ‘container’로 보는 것 입니다. Nx에서는 80/20 접근 방식(libs80/apps20)을 따른다고 [문서](https://nx.dev/more-concepts/applications-and-libraries#mental-model)에 나와있습니다.

라이브러리는 반드시 따로 빌드할 필요는 없습니다. application(‘container’)에서 사용된다면 application 자체에서 빌드됩니다. 따라서 순수한 배포 관점에서는 아무것도 변경되지 않습니다.

즉, 현재 Nx 작업공간 내에서 특정 라이브러리를 사용할 수 있을 뿐만 아니라 패키지 저장소(npm 등)에 publish할 libraries들을 위해 'buildable libraries'들을 만드는 것이 가능합니다.

# 결론
apps와 libs 구분은 보다 모듈화된 아키텍처를 가질 수 있도록 하기 위해서 구분합니다. apps는 libs를 조립하는 container, libs는 하나의 모듈이라고 생각하면 될 것 같습니다.

***
**publish하지 않는 라이브러리**도 libs(package)의 프로젝트로 만들어도 괜찮습니다.

또한, **publish 가능한 라이브러리**도 만들 수 있습니다.