---
title: TypeScript
date: 2019-07-26 17:18:47
tags:
---

# TypeScript

TypeScript 是 JavaScript 的超集

## 高级类型

- 与

``` ts
interface x {
a: string;
b: number;
}

interface y {
  b: number;
  c: string;
}

const z: x & y = {
  a: 'hi',
  b: 233,
  c: 'hello'
}
```

- 或

``` ts
interface x {
  a: string;
  b: number;
}

interface y {
  b: number;
  c: string;
}

const z1: x | y = {
  a: 'hi',
  b: 666
}
const z2: x | y = {
  b: 233,
  c: 'hello'
}
```

- 类型别名 typegraphql 
为类型起一个新的名字

```ts
type str = string;
const name:str = 'jay';
```

- 字面量类型

```ts
type gender = 'male' | 'female';
interface human {
  gender: 'male' | 'female';
}
```

- this 类型

```js
// 指定 this 的类型
function sayHi (this: number, name: string) {
  console.log(`hi, ${name}`);
}
sayHi('jay') // error
sayHi.call(666, 'jay')  // TS 不会检查 call 的类型

function sayHi (this: number | void, name: string) {
  console.log(`hi, ${name}`);
}
sayHi('jay')
```

- 索引类型

```ts
function createObject (options: CreateObjectOptions) {

}
interface CreateObjectOptions {
  [K: string]: any; // 对 key 和 value 进行类型判断
}
createObject({
  obj: {},
  name: 'jay',
  xxx: 123
})

function pluck<T, K extends keyof T>(object: T, keys: Array<K>): T[k][] {
  // T 等于  {name: String, age: Number}
  // keyof T 等于  'name' | 'age'
  // K extends keyof T 等于  'name' | 'age'
  // extends keyof 表示有其中之一的 key，in keyof 表示全部的 key 都要有
  return keys.map(key => object[key])
}
pluck({ name: 'jay', age: 18}, ['name', 'age'])
```

- readonly

```ts
interface Person {
  name: string;
  age: number;
}
const p1: Person = {
  name: 'jay',
  age: 18
}
p1.age = 20;
const p2: Readonly<Person> = {
  name: 'jay',
  age: 18
}
p2.age = 22 // error
```

- partial

```ts
interface Person {
  name: string;
  age: number;
}
type Pserson2 = Partial<Person>;
// Person2 为 Person3 的简写
interface Person3 {
  name?: string;
  age?: number;
}
```

- 可识别类型

  1.有一个共有的字段
  2.共有字段是可穷举的

```ts
type Props = {
  status: false;
  name: string;
  age: number;
} | {
  status: true;
  name: string;
}

const example: Props = {
  status: false,
  name: 'jay',
  age: 18
}
const example2: Props = {
  status: true,
  name: 'jay'
}
function fn (params: Props) {
  if (params.status === true) {
    console.log(params.name);
  } else {
    console.log(params.age);
  }
}
```
