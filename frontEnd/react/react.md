# React

## Class component 和 Function component之间，选哪个更好?
(class和function component之间的区别)[https://overreacted.io/zh-hans/how-are-function-components-different-from-classes/]
这两个之间的区别除了lifecycle管理上的不同，还有一个最重要的就是class component在class中所调用的值是可变的，随着component的改变class中的值会相应的改变。这在某种程度上让class component的state变得