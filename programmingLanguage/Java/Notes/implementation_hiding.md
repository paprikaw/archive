# Implementation hiding

## 包

任意名字的class组件被庇护在唯一的包名字（这个名字一般是域名），在import的时候如果有冲突的话就需要独立import
### Java的编译不依赖linker

Java 不实用连接器将很多文件编译成一个程序，在 Java 中，可运行程序是一组 .class 文件，它们可以打包压缩成一个 Java 文档文件（JAR，使用 jar 文档生成器）。Java 解释器负责查找、加载和解释这些文件。

### 包管理

一个 Java 源代码文件称为一个编译单元（compilation unit）（有时也称翻译单元（translation unit））。每个编译单元的文件名后缀必须是 .java。类库是一组类文件。每个源文件通常都含有一个 public 类和任意数量的非 public 类，因此每个文件都有一个 public 组件。如果把这些组件集中在一起，就需要使用关键字 package。

### Class path

## 访问修饰符（access specifier）

访问控制权限的等级，从“最大权限”到“最小权限”依次是：public，protected，包访问权限（package access）（没有关键字）和 private。

Note: 每个编译单元中只能有一个 public 类，否则编译器不接受。

