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

### Day 03——2022.4.16

#### 1.IDEA上提交代码到GitHub

详情见[五、在IDEA中使用GIt版本控制并将本地代码上传至Github - 纯新手 - 博客园 (cnblogs.com)](https://www.cnblogs.com/alone-striver/p/7745744.html)

* 安装Git
* 创建GitHub仓库
* 在IDEA中配置Git
* IDEA项目引入VCS
* 本地Repositories上传到GitHub的 Repositories

### Day 04——2022.4.19

#### 1.明确需求

* 标签列表
* 话题列表
* 热门列表
* 导航列表
* 登录
* 搜索

#### 2.初识Bootstrap

[Bootstrap中文网 (bootcss.com)](https://www.bootcss.com/)

将解压后的css、js、fonts三个文件夹复制到resources文件夹中，通过直接复制引用index.html文件来快速创建一个导航栏

#### 3.使用GitHub登录

[应用 - GitHub Docs](https://docs.github.com/cn/developers/apps)

进入github在最下方找到API进入应用找到[构建 OAuth 应用程序](https://docs.github.com/cn/developers/apps/building-oauth-apps)。

### Day 05——2022.4.24

#### 1.GitHub登录流程

[授权 OAuth 应用程序 - GitHub Docs](https://docs.github.com/cn/developers/apps/building-oauth-apps/authorizing-oauth-apps)

![1650732763808.png](image/Note/1650732763808.png)

#### 2GitHub登录——调用authorize(1.2)

通过对index.html里登录按钮处连接的设置访问GitHub

先添加地址

```
https://github.com/login/oauth/authorize
```

再根据创建好的 OAuth Apps 的详细信息来完成链接![1650734131603.png](image/Note/1650734131603.png)![1650733972212.png](image/Note/1650733972212.png)![1650733975791.png](image/Note/1650733975791.png)

完成后：

```html
<a href="https://github.com/login/oauth/authorize?client_id=b0995866302b2547236b&redirect_uri=http://localhost:8887/callback&scope=user&state=1">~登录(L_G)
```

tips:地址输入后后续内容通过 ? 来分隔，后续内容通过 & 来区分

#### 3.GitHub登录——获取code(1.2.1)

![1650735813024.png](image/Note/1650735813024.png)

在src/main/java/com/example/community/controller包里新建AuthorizeController.java用来接收redirect_uri从GitHub中携带回来的code

通过加@Controller标签识别Controller的使用

在callback()方法中引入从GitHub中传回的code和state，最后返回到 index.html 页面

```java
package com.example.community.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class AuthorizeController {
    @GetMapping("/callback")
    public String callback(@RequestParam(name = "code") String code,
                           @RequestParam(name = "state") String state) {
        return "index";
    }
}
```

### Day 06——2022.4.27

通过使用OKHTTP来完成登录

[OkHTTP官网](https://square.github.io/okhttp/)

![1650735978202.png](image/Note/1650735978202.png)

#### 1.GitHub登录——用code调用access_token

* 在community包下新建提供服务的provider包，用来提供对第三方支持的代码。
* 在provider下新建GitHubProvider，通过[@Componen](https://blog.csdn.net/weixin_44203158/article/details/109320063)注解来提供Spring的ioc功能

```java
@Component
public class GithubProvider {

}
```

[Spring ioc详解](https://blog.csdn.net/qq_41150890/article/details/108087393)

* 要操作多个参数的时候，需要创建新的对象来储存

![1651115281079.png](image/Note/1651115281079.png)

所以在community包下新建dto（Date Transfer Object）包，在dto包下新建AccesstokenDTO，将需要的参数添加进去并添加构造方法和getter，setter方法：

```java
package com.example.community.dto;

public class AccessTokenDTO {
    private String client_id;
    private String client_secret;
    private String code;
    private String client_uri;
    private String state;

    public String getClient_id() {
        return client_id;
    }

    public void setClient_id(String client_id) {
        this.client_id = client_id;
    }

    public String getClient_secret() {
        return client_secret;
    }

    public void setClient_secret(String client_secret) {
        this.client_secret = client_secret;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getClient_uri() {
        return client_uri;
    }

    public void setClient_uri(String client_uri) {
        this.client_uri = client_uri;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    public AccessTokenDTO(String client_id, String client_secret, String code, String client_uri, String state) {
        this.client_id = client_id;
        this.client_secret = client_secret;
        this.code = code;
        this.client_uri = client_uri;
        this.state = state;
    }

    public AccessTokenDTO() {
    }
}
```

#### 2.GitHub登录——用okhttp来提供获取GitHub的access_token的服务(1.2.1.1~1.2.1.2)

先通过maven上的OK HTTP相关版本的引用代码来让项目配置依赖，在pom.xml文件中引用代码来搜索依赖

最好选用3.14.1版本以防出现问题。

```xml
<dependency>
    <groupId>com.squareup.okhttp3</groupId>
    <artifactId>okhttp</artifactId>
    <version>3.14.1</version>
</dependency>
```

在OKHTTP的官网通过Post to a Server服务来获取登录

![1651155535235.png](image/Note/1651155535235.png)

在GitHubProvider文件中添加getAccessToken方法来获取项目登录应用的access token

注：返回的字符串原本不只有access token的部分：![1651206994447.png](image/Note/1651206994447.png)

需要对字符串进行split拆分再返回

```java
@Component
public class GithubProvider {
    public String getAccessToken(AccessTokenDTO accessTokenDTO) {
        MediaType mediaType
                = MediaType.get("application/json; charset=utf-8");

        OkHttpClient client = new OkHttpClient();

        RequestBody body = RequestBody.create(mediaType, JSON.toJSONString(accessTokenDTO));
        Request request = new Request.Builder()
                .url("https://github.com/login/oauth/access_token")		//用此code去换取access token
                .post(body)
                .build();
        try (Response response = client.newCall(request).execute()) {   //access token携带code
            String string = response.body().string();       //返回access token
            return string.split("&")[0].split("=")[1];		//截取得到的字符串使其只留下access token的部分
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

代码中报错处全部导入OK HTTP3中的包

其中的url括号内应该填入GitHub文档中给定的地址

![1651158537054.png](image/Note/1651158537054.png)

#### 3.GitHub登录——通过JSON来进行传输携带code的access token

在[Maven Repository(Maven仓库)](https://mvnrepository.com/)中找JSON引用代码段。

搜索fastjson

![1651156871903.png](image/Note/1651156871903.png)

选择最新的版本

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>2.0.1</version>
</dependency>
```

注：

[fastjson的缺陷](https://blog.csdn.net/weixin_45737309/article/details/110359056)

[如何用jackson快速替换fastjson](https://blog.csdn.net/hujkay/article/details/97040048?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&utm_relevant_index=1)

* 加载GithubProvider实例

通过注解@Autowired来直接将实例化好的方法调用

[@Component和@Autowired解释](https://blog.csdn.net/weixin_43919304/article/details/98491759?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&utm_relevant_index=2)

回到AuthorizeController文件，通过上图的三处Cliend_id,Cliend_secret和url以及获取到的code和state新创建一个AccessTokenDTO对象（用有参构造）

然后调用第二步中创建好的getAccessToken方法去传输带有临时code和state的access token到GitHub上去，若access token正确则返回真正的access token

```
@Controller
public class AuthorizeController {

    @Autowired
private GithubProvider githubProvider;
    @GetMapping("/callback")
    public String callback(@RequestParam(name = "code") String code,
                           @RequestParam(name = "state") String state) {
        AccessTokenDTO accessTokenDTO = new AccessTokenDTO("b0995866302b2547236b","909f05b68d3db5620291064eb3fa763dbd74d8e4",
                code,"http://localhost:8887/callback",state);
        String accessToken = githubProvider.getAccessToken(accessTokenDTO);
        return "index";
    }
}
```

![1651160606414.png](https://file+.vscode-resource.vscode-webview.net/d%3A/ZFiend/Project/IDEA/StudentCommunity/community/image/Note/1651160606414.png)![1651160613587.png](https://file+.vscode-resource.vscode-webview.net/d%3A/ZFiend/Project/IDEA/StudentCommunity/community/image/Note/1651160613587.png)

#### 4.GitHub登录——获取user信息(1.2.1.3)

* user携带access token去GitHub，若access token正确则返回user信息

在dto包下新建存放user的数据的GitHubUser文件

```java
package com.example.community.dto;

public class GithubUser {
    private String name;
    private Long id;
    private String bio;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getBio() {
        return bio;
    }

    public void setBio(String bio) {
        this.bio = bio;
    }
}
```

需要在GitHubProvider里新增获取user信息的方法getUser

* 以下是okhttp中提供的方法：

![1651206040671.png](image/Note/1651206040671.png)

```java
public GithubUser getUser(String accessToken) {
    OkHttpClient client = new OkHttpClient();

    Request request = new Request.Builder()
            .url("https://api.github.com/user")
            .header("Authorization","token "+accessToken)
            .build();

    try {
        Response response = client.newCall(request).execute();
        String string = response.body().string();
        GithubUser githubUser = JSON.parseObject(string, GithubUser.class);	//将用户信息返回成json文件
        return githubUser;
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

注：现在GitHub已经不支持这样访问api了需要用上述代码来实现详情见[GitHub安全访问api](https://niter.cn/p/115)

最后在AuthorizeController文件内通过获取到的accessToken来调用getUser方法，在其中用JSON获取GithubUser类的对象.

```java
@Controller
public class AuthorizeController {

    @Autowired
private GithubProvider githubProvider;
    @GetMapping("/callback")
    public String callback(@RequestParam(name = "code") String code,
                           @RequestParam(name = "state") String state) {
        AccessTokenDTO accessTokenDTO = new AccessTokenDTO("b0995866302b2547236b","909f05b68d3db5620291064eb3fa763dbd74d8e4",
                code,"http://localhost:8887/callback",state);
        String accessToken = githubProvider.getAccessToken(accessTokenDTO);
        GithubUser user = githubProvider.getUser(accessToken);
        return "index";
    }
}
```

### Day 07——2022.4.29

#### 1.GitHub登录——阶段总结

![1651237657617.png](image/Note/1651237657617.png)

点击登录——>通过GitHub的authorize接口跳转回callback地址并且携带了code——>调用GitHub的access_token接口携带code获取到access_token

‘——>调用GitHub的user接口就能返回user信息

#### 2.配置文件的配置

将之后所需要的一些数据全部写入resources里的application.properties文件里

```properties
# 应用名称
spring.application.name=community
# 应用服务 WEB 访问端口
server.port=8887

github.client.id=b0995866302b2547236b
github.client.secret=909f05b68d3db5620291064eb3fa763dbd74d8e4
github.redirect.uri=http://localhost:8887/callback
```

之后返回authorize文件通过新的注解@Value("${key}")来导入配置文件中的数据内容并替换原先的字符串数据

```java
package com.example.community.controller;

import com.example.community.dto.AccessTokenDTO;
import com.example.community.dto.GithubUser;
import com.example.community.provider.GithubProvider;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;


@Controller
public class AuthorizeController {

    @Autowired
private GithubProvider githubProvider;
    @Value("${github.client.id}")
    private String clientId;
    @Value("${github.client.secret}")
    private String clientSecret;
    @Value("${github.redirect.uri}")
    private String redirectUri;

    @GetMapping("/callback")
    public String callback(@RequestParam(name = "code") String code,
                           @RequestParam(name = "state") String state) {
        AccessTokenDTO accessTokenDTO = new AccessTokenDTO(clientId,clientSecret,
                code,redirectUri,state);
        String accessToken = githubProvider.getAccessToken(accessTokenDTO);
        GithubUser user = githubProvider.getUser(accessToken);
        System.out.println(user.getName());
        return "index";
    }
}
```


## 补充内容
