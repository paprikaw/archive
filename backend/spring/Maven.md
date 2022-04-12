# Maven 基本用法



## maven package的一些配置
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.4</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>hello.HelloWorld</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

* <modelVersion>. POM model version (always 4.0.0).
  * *POM*:  page object modal

* <groupId>. Group or organization that the project belongs to. Often expressed as an inverted domain name.
  * 版本号的使用上，可以参考 https://semver.org/
* <artifactId>. Name to be given to the project’s library artifact (for example, the name of its JAR or WAR file).

* <version>. Version of the project that is being built.

* <packaging> - How the project should be packaged. Defaults to "jar" for JAR file packaging. Use "war" for WAR file packaging.

## Dependencies的申明

``` xml
<dependencies>
		<dependency>
			<groupId>joda-time</groupId>
			<artifactId>joda-time</artifactId>
			<version>2.9.2</version>
		</dependency>
</dependencies>
```


  * <groupId> - The group or organization that the dependency belongs to.

  * <artifactId> - The library that is required.

  * <version> - The specific version of the library that is required.

  * By default, all dependencies are scoped as compile dependencies. That is, they should be available at compile-time (and if you were building a WAR file, including in the /WEB-INF/libs folder of the WAR). Additionally, you may specify a <scope> element to specify one of the following scopes:

  * provided - Dependencies that are required for compiling the project code, but that will be provided at runtime by a container running the code (e.g., the Java Servlet API).

  * test - Dependencies that are used for compiling and running tests, but not required for building or running the project’s runtime code.

