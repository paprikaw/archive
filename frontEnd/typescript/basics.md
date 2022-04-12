# 基本

https://www.tslang.cn/docs/handbook/variable-declarations.html

## Null 和 Undefined

默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

然而，当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 这能避免 很多常见的问题。 也许在某处你想传入一个 string或null或undefined，你可以使用联合类型string | null | undefined。 再次说明，稍后我们会介绍联合类型。

## Never
Never表示一些永远无法取得的值，比如函数如果抛出异常的话，那么它就永远不会有返回值

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

``` ts
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```


## Object

object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

``` ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

## 类型断言
* 类型断言可以帮我们绕开类型检查，比如
``` ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });
```
注意传入createSquare的参数拼写为colour而不是color。 在JavaScript里，这会默默地失败。

你可能会争辩这个程序已经正确地类型化了，因为width属性是兼容的，不存在color属性，而且额外的colour属性是无意义的。

然而，TypeScript会认为这段代码可能存在bug。 如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。

绕开这些检查非常简单。 最简便的方法是使用类型断言：
``` ts
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```
## var申明（过时）

var可以被多次声明，并且没有屏蔽的功能，在不同块作用域中可以共同更改var的值：
``` ts
function sumMatrix(matrix: number[][]) {
    var sum = 0;
    for (var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (var i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }

    return sum;
}
```
以上这个例子，var在外层循环中已经被申明了，但是里层的重复申明导致外层的var也被改变，从而导致错误。


函数在申明的时候会保存其所在的变量环境（let var const），但是var在变量环境中是可以一直改变的，而let在变量环境中则不改变, 有点儿像react的class component中this的对象会随时变化, 而function component则不会改变。这就代表，var的行为在某种程度上是不可预测的。

## 闭包

利用变量声明时会创建环境这个特性，我们可以使用闭包来实现函数状态的留存。
``` ts
var a  = 10;
function Add3(){
    var a = 10;
    return function(){
        a++;
        return a;
    };
};
var cc =  Add3();
console.log(cc());
console.log(cc());
console.log(cc());
console.log(a);
```
## 解构
与javascript差不多，但是里面包含了类型的判断

## 默认值

默认值可以让你在属性为 undefined 时使用缺省值：

``` ts
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

## 思考（使用我的思维来编程时有什么预设条件？）
* 变量自带屏蔽的功能：里层的块对于变量的申明会掩盖外层块的申明
* 变量捕获之后，创建与变量有关的环境，作用域结束之后，这个环境依然是存在的 (不一定)

``` ts
function theCityThatAlwaysSleeps() {
    let getCity;

    if (true) {
        let city = "Seattle";
        getCity = function() {
            return city;
        }
    }

    return getCity();
}
// 在这里，getCity的环境与city的环境在相同的作用域下，即使if代码执行完毕，getcity依然能获得创建getcity时的环境。

```
* 函数式编程时，

## 思考
* 对于知识的不完全掌握是耗费时间的重要因素

     
使用接口定义类型的函数参数会接受额外的检查，有两种方法能绕开这种检查
1. 使用断言（之前提到过）
2. 将参数列表赋给其他对象:
    ``` ts
    interface SquareConfig {
        color?: string;
        width?: number;
    }
    
    function createSquare(config: SquareConfig): { color: string; area: number } {
        // ...
    }
    
    let mySquare = createSquare({ colour: "red", width: 100 });
    ```







