# Reuse

在OOP中总共有两种方法进行代码的复用
1. 第一种方式直接了当。在新类中创建现有类的对象。这种方式叫做“组合”（Composition），通过这种方式复用代码的功能，而非其形式。

2. 第二种方式更为微妙。创建现有类类型的新类。照字面理解：采用现有类形式，又无需在编码时改动其代码，这种方式就叫做“继承”（Inheritance），编译器会做大部分的工作。**继承** 是面向对象编程（OOP）的重要基础之一。更多功能相关将在(Polymorphism）章节中介绍。


## 组合语法

在使用第一种方法的时候，类需要为组合对象初始化，此时会有不同的方法

There are four ways to initialize references:

1. When the objects are defined. This means they’ll always be initialized before the constructor is called.
2. In the constructor for that class.
3. Right before you actually use the object. This is often called lazy initialization. It can reduce overhead in situations where object creation is expensive and the object doesn’t need to be created every time.
4. Using instance initialization.

## 继承语法

### 关于main（）函数的sidenote

我们可以在每一个class里面都放上一个main（），这样有助于测试代码。

Even if you have many classes in a program, the only main() that runs is the one invoked on the command line. So here, when you say java Detergent, Detergent.main() is called. But you can also say java Cleanser to invoke Cleanser.main(), even though Cleanser is not a public class. Even if a class has package access, a public main() is accessible.

即使程序中有很多类都有 main() 方法，惟一运行的只有在命令行上调用的 main()。这里，当你使用 java Detergent 时候，就调用了 Detergent.main()。但是你也可以使用 java Cleanser 来调用 Cleanser.main()，即使 Cleanser 不是一个公共类。即使类只具有包访问权，也可以访问 public main()。

### Super keyword

观察以下这段代码
``` java
public class Detergent extends Cleanser {
  // Change a method:
  @Override
  public void scrub() {
    append(" Detergent.scrub()");
    super.scrub(); // Call base-class version
  }
    ...
}
```
方法scrub是从base object继承过来的，但是在override的时候，现有的代码想用到原来的代码，就可以用`super`关键词

### 子类基类的初始化

必须正确初始化基类子对象，而且只有一种方法可以保证这一点 : 通过调用基类构造函数在构造函数中执行初始化，该构造函数具有执行基类初始化所需的所有适当信息和特权。
``` java
class Art {
  Art() {
    System.out.println("Art constructor");
  }
}

class Drawing extends Art {
  Drawing() {
    System.out.println("Drawing constructor");
  }
}

public class Cartoon extends Drawing {
  public Cartoon() {
    System.out.println("Cartoon constructor");
  }
  public static void main(String[] args) {
    Cartoon x = new Cartoon();
  }
}
/* Output:
Art constructor
Drawing constructor
Cartoon constructor
*/
```
即使不为 Cartoon 创建构造函数，编译器也会为你合成一个无参数构造函数，调用基类构造函数。尝试删除 Cartoon 构造函数来查看这个

### 但参数的构造器初始化
上面的所有例子中构造函数都是无参数的 ; 编译器很容易调用这些构造函数，因为不需要参数。如果没有无参数的基类构造函数，或者必须调用具有参数的基类构造函数，则必须使用 super 关键字和适当的参数列表显式地编写对基类构造函数的调用:
``` java
class Game {
  Game(int i) {
    System.out.println("Game constructor");
  }
}

class BoardGame extends Game {
  BoardGame(int i) {
    super(i);
    System.out.println("BoardGame constructor");
  }
}

public class Chess extends BoardGame {
  Chess() {
    super(11);
    System.out.println("Chess constructor");
  }
  public static void main(String[] args) {
    Chess x = new Chess();
  }
}
/* Output:
Game constructor
BoardGame constructor
Chess constructor
*/
```
## 委托
当一种类会用到另一种类的大多数的方法，但又不是逻辑上的继承关系时，我们可以使用委托

以下是一个宇宙飞船的控制借口：
```java
public class SpaceShipControls {
  void up(int velocity) {}
  void down(int velocity) {}
  void left(int velocity) {}
  void right(int velocity) {}
  void forward(int velocity) {}
  void back(int velocity) {}
  void turboBoost() {}
}

```
使用继承来构建一个宇宙飞船的话，就会出现一个问题：宇宙飞船并不是宇宙飞船控制面板的一类（这在逻辑上是讲不通的）

所以我们可以使用委托：
``` java
public class SpaceShipDelegation {
  private String name;
  private SpaceShipControls controls =
    new SpaceShipControls();
  public SpaceShipDelegation(String name) {
    this.name = name;
  }
  // Delegated methods:
  public void back(int velocity) {
    controls.back(velocity);
  }
  public void down(int velocity) {
    controls.down(velocity);
  }
  public void forward(int velocity) {
    controls.forward(velocity);
  }
  public void left(int velocity) {
    controls.left(velocity);
  }
  public void right(int velocity) {
    controls.right(velocity);
  }
  public void turboBoost() {
    controls.turboBoost();
  }
  public void up(int velocity) {
    controls.up(velocity);
  }
  public static void main(String[] args) {
    SpaceShipDelegation protector =
      new SpaceShipDelegation("NSEA Protector");
    protector.forward(100);
  }
}
```

## 结合组合和继承

## 清理
清理函数必须是初始化函数的相反顺序

## Protected

## final
```java
// reuse/FinalOverridingIllusion.java
// It only looks like you can override
// a private or private final method
class WithFinals {
    // Identical to "private" alone:
    private final void f() {
        System.out.println("WithFinals.f()");
    }
    // Also automatically "final":
    private void g() {
        System.out.println("WithFinals.g()");
    }
}

class OverridingPrivate extends WithFinals {
    private final void f() {
        System.out.println("OverridingPrivate.f()");
    }
    
    private void g() {
        System.out.println("OverridingPrivate.g()");
    }
}

class OverridingPrivate2 extends OverridingPrivate {
    public final void f() {
        System.out.println("OverridingPrivate2.f()");
    } 
    
    public void g() {
        System.out.println("OverridingPrivate2.g()");
    }
}

public class FinalOverridingIllusion {
    public static void main(String[] args) {
        OverridingPrivate2 op2 = new OverridingPrivate2();
        op2.f();
        op2.g();
        // You can upcast:
        OverridingPrivate op = op2;
        // But you can't call the methods:
        //- op.f();
        //- op.g();
        // Same here:
        WithFinals wf = op2;
        //- wf.f();
        //- wf.g();
    }
}

```

输出：

```
OverridingPrivate2.f()
OverridingPrivate2.g()
```
"覆写"只发生在方法是基类的接口时。也就是说，必须能将一个对象向上转型为基类并调用相同的方法（这一点在下一章阐明）。如果一个方法是 **private** 的，它就不是基类接口的一部分。它只是隐藏在类内部的代码，且恰好有相同的命名而已。但是如果你在派生类中以相同的命名创建了 **public**，**protected** 或包访问权限的方法，这些方法与基类中的方法没有联系，你没有覆写方法，只是在创建新的方法而已。由于 **private** 方法无法触及且能有效隐藏，除了把它看作类中的一部分，其他任何事物都不需要考虑到它。


类中所有的 private 方法都隐式地指定为 final。因为不能访问 private 方法，所以不能覆写它。可以给 private 方法添加 final 修饰，但是并不能给方法带来额外的含义。

