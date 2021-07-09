# Eventloop

## Eventloop 的六个阶段

1. timers

2. I/O callbacks

3. idle, prepare

4. poll

5. check

6. colse callbacks



## node 环境

timers 将 setTimeout、setinterval 事件加入 timers 异步队列

poll 等待执行事件 

check 将 setimmediate 事件加入 check 队列

process.nextTick 事件在当前阶段执行后执行可以插队执行

setimmediate > setTimeout > nextTick 



## 宏任务、微任务

 ### 宏任务(Macrotask)

1. setTimeout

2. setinterval

3. requestAnimationFrame

### 微任务 (microtasks)

1. Promise: resolve 会推迟两个时序执行，两个 Promise 会交替执行

2. queueMicrotask

3. MutationObserves

