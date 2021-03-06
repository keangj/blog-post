---
title: TypeScript
date: 2019-07-26 17:18:47
tags: TypeScript
cover: https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/typescript.png
---

# TypeScript

TypeScript 是 JavaScript 的超集

## TypeScript 支持的类型

``` ts
const undefine: undefined = undefined
const e: null = null
const bool: boolean = true
const number: number = 123
const string: string = 'string'
const big: bigint = 123n
const symbol: symbol = Symbol()
const obj: object = {}

// class 可以当作类型使用
class T {}
const a: T = new T()

// null 和 undefined 是所有类型的子类型，所以可以把 null 和 undefined 赋值给其他类型的变量
const bool: boolean = undefined
const number: number = undefined
const string: string = undefined
const big: bigint = undefined
// ...

const bool: boolean = null
const number: number = null
const string: string = null
const big: bigint = null
// ...
```

- any
``` ts
// any 可以是任何值
let any: any = undefined
any = null
any = ''
any = []
```

- 数组

``` ts
const arr: number[] = [1, 2, 3]
const arr2: Array<number | string> = [1, '']	// 泛型
```

- never

``` ts
// 不能将任何值赋给 never
let a: never = 1	// error
let a: never = ''	// error
let a: never = []	// error
let a: never = {}	// error
type T = number & string	// 没有任何值属于 number 和 string 类型，所以 type T = never
```

- unknown

``` ts
// 未知
let a: unknown 
type T = { name: string }
a.name	// 类型“unknown”上不存在属性“name”。ts(2339)
(a as T).name
```

- 函数

``` ts
const fn: (a: number, b: number) => number = (a, b) => a + b
const fn2 = (a: number, b: number) => a + b
type Fn = (a: number, b: number) => number
const fn3:Fn = (a, b) => a + b
interface Fn2 {
  (a: number, b: number): number,
  attr: string
}
const fn4 =  (a: number, b: number) => number
fn4.attr = ''
```

- 元组

``` ts
let arr: [number, string, boolean] = [1, '', true]	// 固定长度数组
```

- 枚举

``` ts
enum Gender {
  Man = 'man',
  Woman = 'woman'
}
let gender: Gender = Gender.Man
console.log(gender)	// man
gender = 'man'	// error
gender = Gender.Woman
console.log(gender)	// woman
```

### 断言

``` ts
let n:any = '123'
console.log((<string>n).split(''))
console.log((n as string).split(''))
```

## 接口

``` ts
// 描述一个对象有哪些属性
interface Human {
  name: string;
  age: number;
  gender: string;
}
const jack: Human = { name: 'jack', age: 18, gender: 'man' } 
const jack: Human = { name: 'jack', age: 18 }	// 类型 "{ name: string; age: number; }" 中缺少属性 "gender"，但类型 "Human" 中需要该属性。ts(2741)

// 可选属性
interface Human {
  name: string;
  age: number;
  gender?: string;
}
const jack: Human = { name: 'jack', age: 18 }

// 接口的继承
interface Animal {
  move(): void;
}
interface Human extends Animal {
  name: string;
  age: number;
}
const jay: Human = {
  name: 'jay',
  age: 1,
  move () {
    console.log('飕飕')
  }
}
jay.move()	// 飕飕
```

## 高级类型

- 交叉类型

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

- 联合类型

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
  readonly age: number;
}

const p1: Person = {
  name: 'jay',
  age: 18
}
p1.age = 20;	// error
p1.name = 'jack'

const p2: Readonly<Person> = {
  name: 'jay',
  age: 18
}
p2.age = 22 // error
p2.name = 'jack'	// error
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

- 泛型

``` ts
type Add<T> = (a: T, b: T) => T
const add: Add<number> = (a, b) => a + b	// 在使用时才确定泛型的类型
```

- 重载

``` ts
function add(a: number, b:number): number
function add(a: string, b:string): string
function add(a: any, b:any): any {}

add(1, 2)
add('hello', 'world')
add('hello', 1)	// error
```

