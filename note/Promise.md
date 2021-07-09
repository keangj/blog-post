

## Promise

Promise.resolve() 创建一个成功或失败

``` js
Promise.resolve(xxx);
Promise.resolve(new Promise((resolve, reject) => reject())); // error
```

Promise.reject() 创建一个失败

``` js
Promise.reject('错误').then(null, reason => console.log(reason))// 错误
```

Promise.all() 所有都成功,或者有一个失败

``` js
Promise.all([Promise.resolve(1), Promise.resolve(2), Promise.resolve(3)])
  .then(values => console.log(values)) // [1, 2, 3]
Promise.all([Promise.resolve(1), Promise.reject(2), Promise.reject(3)])
  .then(null, reason => console.log(reason)) // 2
```

Promise.race() 返回第一个改变状态的结果

``` js
Promise.race([Promise.resolve(1), Promise.resolve(2)])
  .then(values => console.log(values)) // 1
 Promise.race([Promise.reject(1), Promise.resolve(2)])
   .then(null, reason => console.log(reason)) // 1
```

Promise.allSettled() 返回所有的结果

``` js
Promise.allSettled([Promise.reject(1), Promise.resolve(2), Promise.resolve(3)]).then(values => console.log(values)) // [{status: 'rejected', values: 1}, {status: 'fulfilled', values: 2}, {status: 'fulfilled', values: 3}]
```

用 Promise.all()  模拟 allSettled

``` js
const allSettled = promise => promise.then(
  fulfilled => ({status: 'fulfilled', value: fulfilled}),
  rejected => ({status: 'rejected', value: rejected})
)
Promise.all([
  allSettled(Promise.reject(1)), allSettled(Promise.resolve(2)), allSettled(Promise.reject(3))
]).then(values => console.log(values))

// 优化
const allSettled = promiseList => Promise.all(promiseList.map(
  promise => promise.then(
    fulfilled => ({status: 'fulfilled', value: fulfilled}),
    rejected => ({status: 'rejected', value: rejected})
  )
))
allSettled([Promise.reject(1), Promise.resolve(2), Promise.reject(3)])
.then(values => console.log(values))
```

链式调用

``` js
Promise.resolve('hello').then(res => Promise.resolve(`${res}, world`)).then(res => console.log(res))
```

错误处理

``` js
Promise.reject('hello').then(res => console.log(`success:${res}`)).catch(res => console.log(`error:${res}`)) //error:hello
// catch 等同于 .then 的第二个参数
Promise.reject('hello').then(res => console.log(`success:${res}`)).then(null, res => console.log(`error:${res}`))
// 或者
Promise.reject('hello').then(null, res => console.log(`error:${res}`))
```





## async 

await 只能在 async 函数里使用，async 函数会返回一个 Promise

``` js
function fn () {
  return new Promise(resolve => {
    resolve('hi')
  })
}
async function fn2 () {
  let result = await fn()
  console.log(result)	// hi
}
```

try catch 捕获异常

``` js
function ajax () {
  return new Promise((resolve, reject) => {
    reject('出错了')
  })
}
async function fn () {
  try {
  	let result = await ajax()
    console.log(result)
  } catch (error) {
    console.log(`error:${error}`)	// error:出错了
  }
}
fn()
```

使用 catch 捕获异常

``` js
function ajax () {
  return new Promise((resolve, reject) => {
    reject('出错了')
  })
}
async function fn () {
  let result = await ajax().catch(error => console.log(`error:${error}`))	// error:出错了
  console.log(result)
}
fn()
```

await 会使等号左边的代码变成异步

``` js
function fn2 () { 
	console.log('fn2:同步1')
	Promise.resolve('Promise').then(result => console.log(result))
	console.log('fn2:同步2')
} 
async function fn () {
  console.log('同步')
  let result = await fn2()
  console.log('异步')
}
fn()
// '同步'
// 'fn2:同步1'
// 'fn2:同步2'
// 'Promise'
// '异步'
```

## 简单实现 Promise

``` js
class Promise2 {
  queue1 = [] // queue1 为了容纳成功之后的函数们
  queue2 = [] // queue2 为了容纳失败之后的函数们
  constructor(fn){ // new Promise2(fn)
    const resolve = (data)=>{ // fn 接受 resolve 并在成功的时候调用
      setTimeout(()=>{ // 要等一会，否则 queue1 和 queue2 为空
        for(let i =0;i<this.queue1.length;i++){
          this.queue1[i](data)
        }
      })
    }
    const reject = (reason)=>{
      setTimeout(()=>{
      for(let i =0;i<this.queue2.length;i++){
          this.queue2[i](reason)
        }
      })
    }
    
    fn(resolve, reject)
  }
  then(s, e){
    this.queue1.push(s)
    this.queue2.push(e)
    return this
  }
}
p1 = new Promise2((resolve, reject)=>{
  console.log('hi2'); 
  if(Math.random()>0.5){
    resolve('大')
  }else{
    reject('小')
  }
})
p1.then((data)=>{console.log('成功')},  (reason)=>{console.log('失败')})
.then(()=>{console.log('成功2')},  ()=>{console.log('失败2')})
.then(()=>{console.log('成功3')},  ()=>{console.log('失败3')})
```

