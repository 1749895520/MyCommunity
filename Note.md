# 论坛系统项目学习架构日志

---

## 主要内容

### Day 01——2022.4.14

#### 1.Maven安装与配置

* [Maven – Welcome to Apache Maven](https://maven.apache.org/index.html "官网下载链接")————Maven官网下载

![1650012286024.png](image/Note/1650012286024.png)![1650012333756.png](image/Note/1650012333756.png)

* 配置Maven的setings.xml文件
  * 解压maven的压缩文件后进入conf文件夹打开setings.xml文件进行配置。
  * 配置maven本地仓库

    * 在任意位置创建一个maven本地仓库文件夹（repository）

      ```xml
      <localRepository>D:\app\IDEA\Environment\maven\apache-maven-3.6.3\maven-repo</localRepository>

      ```

      将此处setings.xml的 `<localRepository></localRepository>内改成仓库文件夹的地址`
  * 配置阿里云maven镜像

    * 为了让下载速度更快在 `<mirrors></mirrors>之间添加下列代码`

    ```xml
        <mirror>
          <id>alimaven</id>
          <name>aliyun maven</name>
          <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
          <mirrorOf>central</mirrorOf>
        </mirror>


    ```

    ---
  * 配置maven全局JDK版本

    * 在 `<profile></profile>之间添加JDK版本号的代码`
      ```xml
          <!-- java版本 -->
          <profile>
            <id>jdk-1.8</id>
            <activation>
              <activeByDefault>true</activeByDefault>
              <jdk>1.8</jdk>
            </activation>

            <properties>
              <maven.compiler.source>1.8</maven.compiler.source>
              <maven.compiler.target>1.8</maven.compiler.target>
              <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
            </properties>
          </profile>

      ```

### Day 02——2022.4.15

#### 1.Spring Boot项目创建与文件内容

* 1.创建Spring Boot项目

直接在IDEA中创建Spring项目，并等待项目依赖安装完成

* pom.xml（Project Object Model）

Maven中的文件用于更快更便捷的管理各种文件，配置spring的项目包

* .gitignore

用于配置不需要放在GitHub托管的文件

* Application.java

可以直接运行Spring项目

#### 2.Spring Boot项目的运行

* spring.io/guides

Spring 官方指示文档，可以直接在这个网站找到组件如何构建

* 使用 Serving Web Content with Spring MVC组件
  * 导入依赖包
    ```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
            </dependency>
    ```
* 略
  详情去[Getting Started | Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/) 官网组件使用教程

#### 3.访问页面

进入	[Getting Started: Serving Web Content](http://localhost:8080/hello?name=ZFiend)(localhost:8080/hello?name=ZFiend) 页面访问新创建的页面。

其中hello为HTML文件名，name=后面的为任何单词。

#### 4.更改端口号

通过 application.properties 文件来修改一些项目设置，可以直接在其中修改端口号，端口号改为8887

```properties
server.port=8887
```

## 补充内容
