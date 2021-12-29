# ES6标准

## 继承

ES5的继承，实质上是先创造子类的实例对象this，然后再将父类的方法添加到this上（Parent.call(this)）.

ES6的继承，需要先创建父类的this，子类调用super继承父类的this对象，然后再加工。

###  this和super
```
this关键词指向函数所在的当前对象

super指向的是当前对象的原型对象
```

### super 语法糖
```
super.name  
等同于
Object.getPrototypeOf(this).name【属性】  
等同于   
Object.getPrototypeOf(this).name.call(this)【方法】
```

### .super中的this指向

**ES6 规定，在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例。此时的上下文仍还在子类实例当**

``` js
const person = {
    age:'20多了',
    name(){
        return this.age;
    }
}

const man = {
    age:'18岁了',
    sayName(){
        return super.name();
    }
}

Object.setPrototypeOf( man, person );

let n = man.sayName();

console.log( n )    //18岁了
```

### 静态方法中super的指向

super 在静态方法之中指向父类，在普通方法之中指向父类的原型对象

``` js
class Parent {
    static myMethod(msg) {
        console.log('static', msg);
    }
    myMethod(msg) {
        console.log('instance', msg);
    }
}
class Child extends Parent {
    static myMethod(msg) {
        super.myMethod(msg);
    }
    myMethod(msg) {
        super.myMethod(msg);
    }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2
```