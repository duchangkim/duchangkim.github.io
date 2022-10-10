---
layout: post
title: Nx 모노레포에서 프로젝트간 의존관계를 강제해야 하는 이유
category: monorepo
created_date: 2022-10-11
modified_date: 2022-10-11
author: duchangkim
tags: [monorepo, nx]
search_keyword: [monorepo, nx]
---
***

# 왜?
코드를 응집력 있는 단위(잘 정의된)로 분할하면 작은 조직에서도 수십 개의 앱과 수십, 수백 개의 라이브러리가 만들어집니다. 앱과 라이브러리 모두가 서로 아무런 조건 없이 참조한다면 혼란이 오고, 더이상 관리할 수 없게 될 것입니다.

Nx는 코드 분석을 사용하여 프로젝트가 잘 정의된 공개 API에만 의존할 수 있도록 합니다. 또한 프로젝트가 서로 의존하는 방식에 대한 제약조건을 걸 수 있습니다.

# Tags
Nx는 제약 조건을 표현하기 위해 tag를 제공합니다.

예제를 보면서 Nx에서 제공하는 Tag에 대해 알아봅시다. 먼저 project.json에 태그를 추가합니다. 아래 예제에서는 core:core, core:data, core:di 3개의 태그가 있습니다.
```json5
// core/project.json
{
  // ... more project configuration here
  "tags": ["core:core"]
}
```

```json5
// data/project.json
{
  // ... more project configuration here
  "tags": ["core:data"]
}
```

```json5
// di/project.json
{
  // ... more project configuration here
  "tags": ["core:di"]
}
```

<br>
다음으로는 .eslintrc.json을 업데이트 해야 합니다.

`.eslintrc.json`에서 **"@nrwl/nx/enforce-module-boundaries"** 을 찾아 설정할 수 있습니다.

```json5
{
  "files": ["*.ts", "*.tsx", "*.js", "*.jsx"],
  "rules": {
    "@nrwl/nx/enforce-module-boundaries": [
      "error",
      {
        "allow": [],
        "depConstraints": [
          {
            "sourceTag": "core:core",
            "onlyDependOnLibsWithTags": ["core:core"]
          },
          {
            "sourceTag": "core:data",
            "onlyDependOnLibsWithTags": ["core:core", "core:data"]
          },
          {
            "sourceTag": "core:di",
            "onlyDependOnLibsWithTags": ["core:core", "core:data", "core:di"]
          }
        ]
      }
    ]
  }
},
```

<br>

`core:data`는 아래와 같이 정의되어 있습니다.
```json5
{
  "sourceTag": "core:data",
  "onlyDependOnLibsWithTags": ["core:core", "core:data"]
}
```
`core:data`태그를 가진 프로젝트는 `core:core`와 `core:data`를 가진 프로젝트에만 의존할 수 있음을 뜻합니다.

<br>
태그가 없는 프로젝트는 다른 프로젝트에 종속될 수 없습니다. 하지만 아래 설정을 추가한다면 태그가 없는 프로젝트가 다른 프로젝트에 종속될 수 있습니다.

```json5
{
  "sourceTag": "*",
  "onlyDependOnLibsWithTags": ["*"]
}
```

## 특정 태그가 있는 프로젝트 참조 제한
프로젝트가 **의존할 수 있는 태그**를 지정하면 아래 설정과 같이 옵션 목록이 길어질 수 있습니다. 
```json5
{
  "sourceTag": "core:data",
  // `scope:admin`에만 의존할 수 없다고 설정하고 싶습니다...
  "onlyDependOnLibsWithTags": [
    "core:core",
    "core:data",
    "core:di",
    "scope:shared",
    "scope:utils",
    "scope:core",
    "scope:client"
  ]
}
```

<br>

`notDependOnLibsWithTags` 속성은 의존할 수 없는 태그를 명시적으로 지정합니다.
```json5
{
  "sourceTag": "core:data",
  // `scope:admin`을 제외한 모든 태그를 허용합니다.
  "notDependOnLibsWithTags": ["scope:admin"]
}
```

`notDependOnLibsWithTags`규칙과 `onlyDependOnLibsWithTags` 규칙을 조합하여 특정 유형의 프로젝트를 제한할 수도 있습니다.

# 한계점
[다른 예제](https://velog.io/@lky5697/the-ultimate-clean-architecture-template-for-typescript-projects?utm_source=substack&utm_medium=email)는 각 계층(프로젝트)을 빌드하여, 사용하는 프로젝트 내부의 node_modules에서 참조하여 계층 경계를 강제하는 방식으로 구현되어 있습니다. 이 경우에는 잘못된 의존이 있다면 빌드가 실패하기 때문에 좀 더 강력한 강제가 가능하다는 장점이 있습니다.

Nx에서 권장하는 방식으로(tags + eslint) 프로젝트간 경계를 강화하는 방식은 각 프로젝트를 빌드하지 않고 다른 패키지를 직접 의존하는 방식이기 때문에 개발이 편리하다는 장점이 있습니다.(코드 변경 감시 후 rebuild app) 하지만 이 경우에는 ESLint가 error로 표시해주긴 하지만 빌드가 실패한다거나 하는 강제성이 부족합니다. 커밋 직전에 lint, test해주는 도구의 힘을 빌려서 강제성을 좀 더 강화할 순 있을 것이라 생각하지만 다른 예제의 방식보다는 약하다고 생각됩니다.

# 결론
git commit시 lint error라면 커밋을 하지 못하도록 설정([husky](https://typicode.github.io/husky/#/))하게 된다면 강력하게 강제할 수 있기 때문에 ESLint 설정만으로 충분하다고 판단됩니다.


# 참고 문서
[enforce-project-boundaries](https://nx.dev/core-features/enforce-project-boundaries)