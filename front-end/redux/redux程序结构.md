# redux程序结构

## redux的增删查改

### 增：初始化Store时的逻辑
使用createStore来初始化Store:
``` js
// src/rootReducer.js
import { combineReducers } from 'redux'

import todosReducer from './features/todos/todosSlice'
import filtersReducer from './features/filters/filtersSlice'

const rootReducer = combineReducers({
  // Define a top-level state field named `todos`, handled by `todosReducer`
  todos: todosReducer,
  filters: filtersReducer
})

export default rootReducer
```
``` js
// src/store.js
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension'
import rootReducer from './reducer'

const composedEnhancer = composeWithDevTools(applyMiddleware(thunkMiddleware))

const store = createStore(rootReducer, composedEnhancer)
export default store
```
在createStore的时候可以给Store赋予一个初值:
``` js
import { createStore } from 'redux'
import rootReducer from './reducer'

let preloadedState
const persistedTodosString = localStorage.getItem('todos')

if (persistedTodosString) {
  preloadedState = {
    todos: JSON.parse(persistedTodosString)
  }
}

const store = createStore(rootReducer, preloadedState)
```
这样可以直接将网络接收到的或者本地保存的数据拿来初始store中的state

这里我们首先将所有Reducers combine成一个root reducers，然后在store中配置各种中间件，这样较为繁琐，也可以使用configureStore：
```js
//src/store.js
import { configureStore } from '@reduxjs/toolkit'

import todosReducer from './features/todos/todosSlice'
import filtersReducer from './features/filters/filtersSlice'

const store = configureStore({
  reducer: {
    // Define a top-level state field named `todos`, handled by `todosReducer`
    todos: todosReducer,
    filters: filtersReducer
  }
})

export default store
```
### 删: 同上
### 查: 使用useSelector或者getState
一般访问store中state的方法时我们会使用getState
``` js
// 我们有一个selector function，给一个state取出state中的值
export const selectCount = state => state.counter.value

// 使用selector function需要我们先从store中call getState
const count = selectCount(store.getState())
```
如果使用useSelector
``` js
// 我们可以直接使用selector function
const count = useSelector(selectCount)
```
### 改
使用store.dispatch或useDispatch

import store并使用`store.dispatch()`对state更新或者使用useDispatch进行更新:
``` js
const dispatch = useDispatch();
```

## 创建一个Reducer的捷径
### createSlice()能帮我们做这些
一个基础的createSlice（）像这样：
``` js
import { createSlice } from '@reduxjs/toolkit'

const initialState = []

const todosSlice = createSlice({
  name: 'todos',
  initialState,
  reducers: {
    todoAdded(state, action) {
      // ✅ 这里对state的“mutate”的修改方式在createSlice中是可以的 
      // 解释在这里：createSlice uses a library called Immer inside. Immer uses a special JS tool called a Proxy to wrap the data you provide, and lets you write code that "mutates" that wrapped data. But, Immer tracks all the changes you've tried to make, and then uses that list of changes to return a safely immutably updated value, as if you'd written all the immutable update logic by hand.
      // 注意！ 只能在createSlice和createReducer中使用这种mutate的写法，不然就会出问题
      state.push(action.payload)
    },
    todoToggled(state, action) {
      const todo = state.find(todo => todo.id === action.payload)
      todo.completed = !todo.completed
    },
    todosLoading(state, action) {
      return {
        ...state,
        status: 'loading'
      }
    }
  }
})

export const { todoAdded, todoToggled, todosLoading } = todosSlice.actions

export default todosSlice.reducer
```

思考：在公司的代码中，action，reducer是分开进行维护的。createSlice给了我们一种方便的方法将这两者结合在一起，通过一个api统一的去进行获取。并且在这个过程中我们还可以使用mutate的方法去写reducer function。

### createSlice中的prepare()
createSlice会为我们自动的返回action creator。这个action creator默认action creator的参数为action的payload。但是有的时候我们需要在action creator内对参数进行一些修改在添入payload。这种情况就需要在creaSlice中加入prepare函数
``` js
const postsSlice = createSlice({
  name: 'posts',
  initialState,
  reducers: {
    postAdded: {
      reducer(state, action) {
        state.push(action.payload)
      },
      prepare(title, content) {
        return {
          payload: {
            id: nanoid(),
            title,
            content
          }
        }
      }
    }
    // other reducers here
  }
})
```


## 异步处理
普通的reducer没办法处理异步的需求。我们需要thunk wrap在普通的action creator外面来处理异步需求:

假设我们的action creator是`incrementByAmount(amount)`，我们thunk的code会像：
``` js
export const incrementAsync = amount => dispatch => {
  setTimeout(() => {
    dispatch(incrementByAmount(amount))
  }, 1000)
}
```
我们可以像使用正常action creator一样去使用这个async thunk:
``` js
store.dispatch(incrementAsync(5))
```

需要注意的是，使用thunk需要我们在配置store的时候加上middleware这个配置，是store具有解析thunk的能力

思考：thunk实际上就像一个中转站，在store收到一个thunk dispatch的请求时，store不会直接调用dispatch，而是使用thunk 内部的逻辑去决定何时调用dispatch

## 中间件
关于中间件的理解：
In particular, middleware are intended to contain logic with side effects. In addition, middleware can modify dispatch to accept things that are not plain action objects. We'll talk more about both of these in Part 6: Async Logic.

上面已经说过如何在createStore的时候加入中间件了，我们可以写自己的中间件

## selector inside

如果我们使用`useSelector()`, useSelector会在每次dispatch action之后重新跑一次看一下本次和上次的结果是不是一样的，如果结果不一样，就rerender这个component。这里有一个问题，如果在selector中使用了map之类的函数，每次返回的ref都是不一样的，所以每次action dispatch的时候使用这个selector的component都会更新。

解决方法1:

useSelector的第二个参数接受一个comparison function，我们可以用react-redux中的shadow-equal function来检查ref中的每一个元素是否相同 [这个链接](https://redux.js.org/tutorials/fundamentals/part-5-ui-react#selecting-data-in-list-items-by-id)写了具体操作

解决方法2:

使用Memorize selector

简单的例子:
``` js
/* 
 * All "input selectors" are called with all of the arguments
 * If any of the input selector return values have changed, the "output selector" will re-run
 * All of the input selector results become arguments to the output selector
 * The final result of the output selector is cached for next time
import { createSelector } from 'reselect'
*/

// omit reducer

// omit action creators

export const selectTodoIds = createSelector(
  // First, pass one or more "input selector" functions:
  state => state.todos,
  // Then, an "output selector" that receives all the input results as arguments
  // and returns a final result value
  todos => todos.map(todo => todo.id)
)
```

对于useSelector的思考:
useSelector可以借助createSelector实现一些比较复杂的逻辑，这部分的逻辑我们也可以不写在selector里面（使用）