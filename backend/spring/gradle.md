# gradle 基本用法

## gradle配置文件 gradle.build

``` java
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'application'

mainClassName = 'hello.HelloWorld'

// tag::repositories[]
repositories { 
    mavenCentral() 
}
// end::repositories[]

// tag::jar[]
jar {
    archiveBaseName = 'gs-gradle'
    archiveVersion =  '0.1.0'
}
// end::jar[]

// tag::dependencies[]
sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    implementation "joda-time:joda-time:2.2"
    testImplementation "junit:junit:4.12"
}
// end::dependencies[]

// tag::wrapper[]
// end::wrapper[]
```
* *repositories* block indicates that the build should resolve its dependencies from the Maven Central repository. 
* sourceCompatibility / targetCompatibility: source表示本project的code是用什么版本的java编译的，target表示期望这个编译出来的class能兼容到什么版本
* Dependencies:
  * implementation. Required dependencies for compiling the project code, but that will be provided at runtime by a container running the code (for example, the Java Servlet API).
  * testImplementation. Dependencies used for compiling and running tests, but not required for building or running the project’s runtime code.
  * compile. indicating that it should be available during compile-time
* Jar block: the jar block specifies how the JAR file will be named. In this case, it will render gs-gradle-0.1.0.jar.

## 使用gradle wrapper
The Gradle Wrapper is the preferred way of starting a Gradle build. It consists of a batch script for Windows and a shell script for OS X and Linux. These scripts allow you to run a Gradle build without requiring that Gradle be installed on your system. This used to be something added to your build file, but it’s been folded into Gradle, so there is no longer any need. Instead, you simply use the following command:

```
$ gradle wrapper --gradle-version 6.0.1
```

跑完这个命令后gradle项目目录就变为了：
```
└── <project folder>
    └── gradlew
    └── gradlew.bat
    └── gradle
        └── wrapper
            └── gradle-wrapper.jar
            └── gradle-wrapper.properties
```
现在我们可以使用`gradlew build`来build这个project，这能确保每一个开发者编译项目时使用的是同一个gradle版本

## build 目录

```
build
├── classes
│   └── main
│       └── hello
│           ├── Greeter.class
│           └── HelloWorld.class
├── dependency-cache
├── libs
│   └── gs-gradle-0.1.0.jar
└── tmp
    └── jar
        └── MANIFEST.MF
```

## Runnable
上述的build过程结束后，我们的jar文件中长这样：
``` sh
$ jar tvf build/libs/gs-gradle-0.1.0.jar
  0 Fri May 30 16:02:32 CDT 2014 META-INF/
 25 Fri May 30 16:02:32 CDT 2014 META-INF/MANIFEST.MF
  0 Fri May 30 16:02:32 CDT 2014 hello/
369 Fri May 30 16:02:32 CDT 2014 hello/Greeter.class
988 Fri May 30 16:02:32 CDT 2014 hello/HelloWorld.class
```

很明显，这里面没有包含dependency，这个jar不是runnable的。我们需要加上

```
apply plugin: 'application'
mainClassName = 'hello.HelloWorld'
```

这样我们就能run的起来了
## 将不同种类的项目打包依赖

* If we were building a WAR file, we could use gradle’s WAR plugin. 

* If you are using Spring Boot and want a runnable JAR file, the spring-boot-gradle-plugin is quite handy.



