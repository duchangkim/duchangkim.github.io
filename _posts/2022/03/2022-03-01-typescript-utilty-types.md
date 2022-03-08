---
layout: post
title: 타입스크립트 - 유틸리티 타입 (TypeScript - utility types)
category: TS
created_date: 2022-03-01
modified_date: 2022-03-08
author: duchangkim
tags: [ts]
search_keyword: [ts, type, utility]
---
***

# 타입스크립트의 유틸리티 타입
타입스크립트는 **공통 타입 변환을 용이**하게 하기 위해 몇가지 유틸리티 타입을 제공합니다. 이런 유틸리티 타입은 전역으로 사용 가능합니다.

## 유틸리티 타입 종류
- [`Partial<T>`](#partial)
- [`Readonly<T>`](#readonly)
- [`Record<K,T>`](#record)
- [`Pick<T,K>`](#pick)
- [`Omit<T,K>`](#omit) - To be updated
- [`Exclude<T,U>`](#exclude) - To be updated
- [`Extract<T,U>`](#extract) - To be updated
- [`NonNullable<T>`](#nonnullable) - To be updated
- [`Parameters<T>`](#parameters) - To be updated
- [`ConstructorParameters<T>`](#constructorparameters) - To be updated
- [`ReturnType<T>`](#returntype) - To be updated
- [`InstanceType<T>`](#instancetype) - To be updated
- [`Required<T>`](#required) - To be updated
- [`ThisParameterType`](#thisparametertype) - To be updated
- [`OmitThisParameter`](#omitthisparameter) - To be updated
- [`ThisType<T>`](#thistype) - To be updated

## `Partial<T>`
{: #partial}

Partial(파셜) 타입은
- `T`의 모든 프로퍼티를 선택적으로 만드는 타입을 구성합니다. 
- 주어진 타입의 모든 하위 타입 집합을 나타내는 타입을 반환합니다.

**예제**
```typescript
// User 인터페이스
interface User {
  id: string;
  name: string;
  nickname: string;
  email: string;
  mobile: string;
}

// 기존 User와 업데이트할 User정보를 받아 새로운 User정보를 리턴하는 함수
const updateUser = (user: User, fieldsToUpdate: Partial<User>) => {
  return { ...user, ...fieldsToUpdate };
};

const duchi: User = {
  id: 'duchang1234',
  name: 'duchang',
  nickname: 'du',
  email: 'duchang.dev@gmail.com',
  mobile: '10 xxxx yyyy',
};

const updatedDuchi: User = updateUser(duchi, {
  // Partial type 덕분에 nickname 하나만 전달해도 된다. 
  // (id, name 등등 어떤 것이든 전달인자로 넘길 수 있다.)
  nickname: 'duchi',
});
```
<br />

파셜 타입이 없다면 아래와 같이 인터페이스 하나를 또 정의해야 하는 불편함이 생길 수 있을것 같습니다.
```typescript
// 아래처럼 새로운 인터페이스 하나를 또 정의해야 합니다.
interface UserFieldsToUpdate {
  id?: string;
  name?: string;
  nickname?: string;
  email?: string;
  mobile?: string;
}

const aeong: User = {
  id: 'aeong1234',
  name: 'aeong',
  nickname: 'mya',
  email: 'aeong.dev@mail.com',
  mobile: '10 xxxx yyyy',
};

const updateUser2 = (user: User, fieldsToUpdate: UserFieldsToUpdate) => {
  return { ...user, ...fieldsToUpdate };
};

const updatedAeong: User = updateUser2(aeong, {
  nickname: 'pingpinge',
});
```

[Partial 예제](https://stackblitz.com/edit/typescript-mfz6zi?file=PartialType.ts)

***

## `Readonly<T>`
{: #readonly}

Redonly(리드온리) 타입은
- `T`의 모든 프로퍼티를 읽기전용으로 설정한 타입을 구성합니다.
- 생성된 타입의 프로퍼티는 값을 재할당할 수 없습니다.

**예제**
```typescript
interface Cat {
  name: string;
  like: string;
  age: number;
}

const kuku: Readonly<Cat> = {
  name: 'kuku',
  like: 'box',
  age: 3,
};

// error! 읽기전용 객체이므로 재할당 불가능
kuku.like = 'you';
```

<br />

아래와 같이 사용 가능합니다. (freeze가 반환한 객체에 값을 재할당 하려는 경우를 막는 용도)
```typescript
function freeze<T>(obj): Readonly<T> {
  return Object.freeze(obj);
}

const aeong: Cat = {
  name: 'aeong',
  like: 'catnip',
  age: 5,
};

const readonlyAeong = freeze<Cat>(aeong);
readonlyAeong.age = 6;
```

[Readonly 예제](https://stackblitz.com/edit/typescript-mfz6zi?file=Readonly.ts)

***

## `Record<K, T>`
{: #record}

Record(레코드) 타입은
- 프로퍼티 키가 `K`타입, 값이 `T`타입인 타입을 구성합니다.
- `Record`타입은 타입의 프로퍼티들을 다른 타입에 매핑시키는 데 사용할 수 있습니다.

**예제**
```typescript
interface PageInfo {
  title: string;
  backgroundColor: string;
}

type Page = 'home' | 'about' | 'posts';

const pages: Record<Page, PageInfo> = {
  home: { // type Page의 값
    title: 'Home', // interface PageInfo의 title
    backgroundColor: 'black', // interface PageInfo의 backgroundColor
  },
  about: {
    title: 'About',
    backgroundColor: 'red',
  },
  posts: {
    title: 'Posts',
    backgroundColor: 'gray',
  },
};
```

[Record 예제](https://stackblitz.com/edit/typescript-mfz6zi?file=Record.ts)

***

## `Pick<T, K>`
{: #pick}

Pick(픽) 타입은
- 타입 `T`의 프로퍼티 `K`의 집합을 선택해서 타입을 구성합니다.

```typescript
interface Post {
  title: string;
  tags: string[];
  content: string;
  index: number;
  description: string;
}

// Post 인터페이스에서 title, tags, description 프로퍼티만 골라와서 타입을 구성합니다.
type PostPreview = Pick<Post, 'title' | 'tags' | 'description'>;

const postPreview: PostPreview = {
  title: '야옹이는 어떻게 야옹할까?',
  tags: ['고양이', '야옹'],
  description:
    '야옹이는 어떻게 야옹할까에 대한 글 입니다. 이 글을 읽으시려면 드루오세요',
};
```

[Pick 예제](https://stackblitz.com/edit/typescript-mfz6zi?file=Pick.ts)