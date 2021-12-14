# 基本使用

## React-router-config
我们会使用react-router-config来配置一个router

一个例子
``` js
const routes = [
  {
    component: Root,
    routes: [
      {
        path: "/",
        exact: true,
        component: Home
      },
      {
        path: "/child/:id",
        component: Child,
        routes: [
          {
            path: "/child/:id/grand-child",
            component: GrandChild
          }
        ]
      }
    ]
  }
];
```
我们将route的配置加入到一个object里面，然后使用renderRoutes来实例化这个router
###