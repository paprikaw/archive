# redux toolkit相关用法
## createAsyncThunk
如果是一般的Async操作，我们可以将async操作包裹在async(dispatch, getState) => {}函数里面。但是我们常常需要在async操作的时候追踪async函数的状态（error，loading state）

首先，一般在component里面我们会维护component的状态（比如一个modal，我们需要知道它是否正在拉取数据，数据加载是否成功）。配合createAsyncThunk在AJAX操作后会dispatch三个action来帮我们确认程序AJAX拉取后的状态，加入我们有一个async action users/requestStates:
```
pending: 'users/requestStatus/pending'
fulfilled: 'users/requestStatus/fulfilled'
rejected: 'users/requestStatus/rejected'
```

