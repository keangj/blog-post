# 响应式



## Objct.defineProperty

``` js
function observe(data) {
  if(!data || typeof data !== 'object') return
  for(var key in data) {
    let val = data[key]  
    Object.defineProperty(data, key, {
      enumerable: true,
      configurable: true,
      get: function() {
        track(data, key)
        return val
      },
      set: function(newVal) {
        trigger(data, key, newVal)
        val = newVal
      }
    })
    if(typeof val === 'object'){
      observe(val)
    }
  }
}

function track(data, key) {
  console.log('get data ', key)
}

function trigger(data, key, value) {
  console.log('set data', key, ":", value)
}

var data = {
  name: 'hunger',
  friends: [1, 2, 3]
}
observe(data)

console.log(data.name)
data.name = 'valley'
data.friends[0] = 4
data.friends[3] = 5 // 非响应式
data.age = 6  //非响应式
```

## proxy

``` js
function reactive(obj) {
    const handler = {
      get(target, prop, receiver) {
        track(target, prop)
        const value = Reflect.get(...arguments)
        if (typeof value === 'object') {
          return reactive(value)
        } else {
          return value
        }
      },
      set(target, key, value, receiver) {
        trigger(target, key, value)
        return Reflect.set(...arguments)
      }
    }
    return new Proxy(obj, handler)
  }
  function track(data, key) {
    console.log('get data ', key)
  }

  function trigger(data, key, value) {
    console.log('set data', key, ":", value)
  }


  const dinner = {
    meal: 'tacos'
  }
  const proxy = reactive(dinner)
  proxy.meal = 'apple'
  proxy.list = []
  proxy.list.push(1) //响应式
```

