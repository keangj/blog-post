---
title: Underscore
date: 2019-04-08 23:03:38
tags:
cover: https://images-platform.99static.com/WG4uXLVV_iftAlJttXgvXgU6th0=/500x500/top/smart/99designs-contests-attachments/12/12583/attachment_12583422
---
# Underscore 基本用法

## Collections

### map() 对集合每一项进行一些操作，返回新数组

```js
  _.map([1, 2, 3, 4, 5], i => i+1)    //   [2, 3, 4, 5, 6]
```

### each() 遍历集合类似map

```js
  _.each([1, 2, 3, 4, 5], i => console.log(i+1))  // 2 3 4 5 6
```

### reduce() 对集合进行操作且累计并返回结果，第三个参数为操作初始值

```js
  _.reduce([1, 2 ,3], (memo, num) => memo + num, 0)   // 6
  _.reduce([1, 2 ,3], (memo, num) => memo + num, 10)   // 16
  _.reduce([1, 2 ,3], (memo, num) => memo * num, 1)   // 6
  _.reduce([1, 2 ,3], (memo, num) => memo * num, 10)  // 60
```

### shuffle() 打乱集合顺序

```js
  const arr = [1, 2, 3, 4, 5]
  _.shuffle(arr)  // [3, 4, 2, 1, 5]
```

### sample() 从集合中随机选择一个或指定数目的项

```js
  const arr = [1, 2, 3, 4, 5]
  _.sample(arr) // 2
  _.sample(arr, 2) // [2, 5]
```

### every() 集合的每一个项都符合条件返回true，否则返回false

### some() 集合的某一个项符合条件返回true，否则返回false

```js
    _.every([0, 0, 0 ,0], item => item === 0 )  // true
    _.every([0, 0, 1 ,0], item => item === 0 ) // false
    _.some([0, 0, 0 ,0], item => item === 0 )  // true
    _.some([0, 2, 1 ,0], item => item === 0 )  // true
    _.some([1, 1, 1 ,1], item => item === 0 ) // false
```

### find() 查找集合中符合条件的第一项

### filter() 返回所有符合条件的元素

### reject() 与 filter() 相反

```js
const p = [
    { name: 'Jay', age: 18, gender: 'male' },
    { name: 'Kang', age: 14, gender: 'male'},
    { name: 'Christine', age: 6, gender: 'female' }
]
_.find(p, item=> item.gender==='male' )
// { name: 'Jay', age: 18, gender: 'male' }
_.filter(p, item=> item.gender === 'male' )
// { name: 'Jay', age: 18, gender: 'male'}{name: 'Kang', age: 14, gender: 'male' }
_.reject(p, item=> item.gender==='male' )
// { name: 'Christine', age: 6, gender: 'female' }
```

### where() 返回包含条件的项

### findWhere() 同 where，只返回第一个项

```js
const p = [
    { name: 'Jay', age: 18, gender: 'male' },
    { name: 'Kang', age: 14, gender: 'male'},
    { name: 'Christine', age: 6, gender: 'female' }
]
_.where(p, {name:'Jay'})
// { name: 'Jay', age: 18, gender: 'male' }
_.where(p, {gender: 'male'})
// { name: 'Jay', age: 18, gender: 'male'}{name: 'Kang', age: 14, gender: 'male' }
_.findWhere(p, {gender: 'male'})
// { name: 'Jay', age: 18, gender: 'male' }
```

### contains() 包含指定的值，返回 true

```js
    const arr = [1, 2, 3, 2]
    _.contains(arr, 2)  // true
    _.contains(arr, 4)  // false
```

### pluck() 返回指定 key 的 value

```js
    const p = [
        { name: 'Jay', age: 18, gender: 'male' },
        { name: 'Kang', age: 14, gender: 'male'},
        { name: 'Christine', age: 6, gender: 'female' }
    ]
    _.pluck(p, 'name' ) // [ 'Jay', 'Kang', 'Christine']
```

### max() 返回最大值

### min() 返回最小值

```js
  const p = [
      { name: 'Jay', age: 18, gender: 'male' },
      { name: 'Kang', age: 14, gender: 'male'},
      { name: 'Christine', age: 6, gender: 'female' }
  ]
  _.max(p, item=> item.age )  // { name: 'Jay', age: 18, gender: 'male' }
  _.min(p, item=> item.age )  // { name: 'Christine', age: 6, gender: 'female' }
```

### groupBy() 按 key 把一个集合分成多个集合

```js
    const p = [
        { name: 'Jay', age: 18, gender: 'male' },
        { name: 'Kang', age: 14, gender: 'male'},
        { name: 'Christine', age: 6, gender: 'female' }
    ]
    _.groupBy(p, item=> item.gender )   
    // { female: [{ name: 'Christine', age: 6, gender: 'female' }],
    //   male: [{ name: 'Jay', age: 18, gender: 'male' }, { name: 'Kang', age: 14, gender: 'male'}] }
```

### indexBy() 返回以指定 key的 value 分类的对象

```js
    const p = [
        { name: 'Jay', age: 18, gender: 'male' },
        { name: 'Kang', age: 14, gender: 'male'},
        { name: 'Christine', age: 6, gender: 'female' }
    ]
    _.groupBy(p, 'gender' ) 
    // { female: [{ name: 'Christine', age: 6, gender: 'female' }],
    //   male: [{ name: 'Jay', age: 18, gender: 'male' }, { name: 'Kang', age: 14, gender: 'male'}] }
```

### countBy() 返回依据条件分类的每一类的数目

```js
    const p = [
        { name: 'Jay', age: 18, gender: 'male' },
        { name: 'Kang', age: 14, gender: 'male'},
        { name: 'Christine', age: 6, gender: 'female' }
    ]
    _.countBy(p, item=> item.age > 10 ? '小学僧' : '中学僧' ) // { '小学僧': 2, '中学僧': 1 }
```

### size() 返回集合长度

```js
    _.size([1,2,3,4,5]) // 5
```

### partition() 返回数组包含两个数组，第一个符合条件，第二个不符合条件

```js
    _.partition([1, 2, 3, 4, 5], i => i > 2) // [ [3, 4, 5], [1, 2] ]
```

## 数组(Arrays)

- first() 返回数组的第一个元素
- initial() 返回除最后一个(或多个)以外的元素
- last() 返回数组的最后一个元素
- rest() 返回除第一个(或多个)以外的元素

    ```js
        _.first([1, 2, 3, 4, 5])  // 1
        _.first([1, 2, 3, 4, 5], 2)  // [1, 2]
        _.initial([1, 2, 3, 4, 5])  // [1, 2, 3, 4]
        _.last([1, 2, 3, 4, 5])  // [5]
        _.rest([1, 2, 3, 4, 5])  // [2, 3, 4, 5]
    ```

- compact() 返回不包含 false 值的数组

    ```js
    _.compact([0, 1, 2, false, '', null, undefined])  // [1, 2]
    ```

- flatten() 将多层嵌套的数组转化成减少至一层,传递第二个参数为 true 就只减少一层

    ```js
    _.flatten([1, [2, [3]]])  // [1, 2, 3]
    _.flatten([1, [2, [3]]], true)  // [1, 2, [3]]
    ```

- without() 返回一个不包含指定值的数组

    ```js
    _.without([0, 1, 2, 3, 4, 4], 3, 4)  // [0, 1, 2]
    ```

- union() 返回传入数组的并集

    ```js
    _.union([1, 2, 3],[3, 4, 5], [7, 6, 5])  // [1, 2, 3, 4, 5, 7, 6]
    ```

- intersection() 返回传入数组的交集

    ```js
    _.intersection([1,2,3,6],[3,4,5,6],[6,5,4,3,2])  // [3, 6]
    ```

- difference() 接受两个数组，返回第一个数组中不包含第二个数组的值的数组、

    ```js
    _.difference([1,2,3,6],[3,4])  // [1, 2, 6]
    ```

- uniq() 返回一个去重后的新数组

    ```js
    _.uniq([6,1,2,3,4,6,5,5,4])  // [6, 1, 2, 3, 4, 5]
    ```

- zip() 将接收数组的值按对应的下标合并

    ```js
    _.zip(['k', 'j', 'hi'],[3, 1, 2])  // [['k', 3], ['j', 1], ['hi', 2]]
    ```

- object() 合并数组为对象，如 key 重复就返回最后一个值

    ```js
    _.object(['name', 'age'],['jay', 18])  // {name: "jay", age: 18}
    _.object(['name', 'age', 'age'],['jay', 18, 14])  // {name: "jay", age: 14}
    ```

- indexOf() 返回指定值在数组中的下标

- lastIndexOf() 返回指定值在数组中从后开始的下标

    ```js
    _.indexOf([1, 'hi', 'jay'], jay)  // 2
    _.indexOf([1, 'hi', 'jay'], jay)  // 0
    ```

- range() 接收一个开始数字和一个结束数字，生成一个数字数组

    ```js
    _.range(5)  // [0, 1, 2, 3, 4]
    _.range(0, -10, 3)  // [0, -3, -6, -9]
    _.range(0, 10, 3)  // [0, 3, 6, 9]
    ```