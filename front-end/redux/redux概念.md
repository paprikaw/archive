# Redux 概念

## basics
### State

一个正常的state是像这样的
``` js
function Counter() {
  // 一个状态，一个修改状态的函数 
  const [counter, setCounter] = useState(0)

  // 改变state的action，在这里是一个函数 
  const increment = () => {
    setCounter(prevCounter => prevCounter + 1)
  }

  // View，由react进行渲染 
  return (
    <div>
      Value: {counter} <button onClick={increment}>Increment</button>
    </div>
  )
}
```
很明显，这个过程就像一个循环

* Note:当修改state时，不能直接修改存储state的数据结构，我们需要改变state的指向
``` js
const obj = {
  a: {
    // To safely update obj.a.c, we have to copy each piece
    c: 3
  },
  b: 2
}

const obj2 = {
  // copy obj
  ...obj,
  // overwrite a
  a: {
    // copy obj.a
    ...obj.a,
    // overwrite c
    c: 42
  }
}
```
在写类似的复制语句时必须得思考什么时候复制，什么时候overwrite

## 一些概念

action:
```
You can think of an action as an event that describes something that happened in the application.

一个action由两个field组成:
1: type
2: payload
}
```

``` js
const action = {
  type: 'someActionName',
  payload: 'Some things to hold'
```
Action Creators​:
我们用这种function快速创建一个action：
``` js
const addAction = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```

Reducers:
Reducer接收到现有的state和action，将action实施到state上，可以理解为：
``` js
(state, action) => newState
```
具体例子:
``` js
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

Store:
Redux将state存储在一个object中，这个object的初始化需要接受reducers
```js
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```
Dispatch：作为User的我们唯一修改State的方法就是使用Dispatch方法
```js
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState())
// {value: 2}
```

Selectors​:
Selectors就是一个知道如何提取特定state数据的方法，这个主要是避免多个位置访问同一个state数据区域时逻辑上的重复

Redux的flow：
```
User创建Reducer（同时定义了不同的state） => 
程序利用Reducer创建了Store, 将所有的state初始化 => 
App会使用dispatch实施不同的action like: dispatch(action) =>
根据不同的action，用reducer中对应的方法去更改store中储存的state => 
订阅发布，UI更新
```

