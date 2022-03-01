---
layout: post
title: 타입스크립트 - 유틸리티 타입 (TypeScript - utility types)
category: TS
created_date: 2022-03-01
modified_date: 2022-03-01
author: duchangkim
tags: [ts]
search_keyword: [ts, type]
---
***

# 타입스크립트의 유틸리티 타입
타입스크립트는 **공통 타입 변환을 용이**하게 하기 위해 몇가지 유틸리티 타입을 제공합니다. 이런 유틸리티 타입은 전역으로 사용 가능합니다.

## 유틸리티 타입 종류
- [`Partial<T>`](#partial)
- [`Readonly<T>`](#readonly) - To be updated
- [`Record<K,T>`](#record) - To be updated
- [`Pick<T,K>`](#pick) - To be updated
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

파셜 타입은
- `T`의 모든 프로퍼티를 선택적으로 만드는 타입을 구성합니다. 
- 주어진 타입의 모든 하위 타입 집합을 나타내는 타입을 반환합니다.

<br />

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

