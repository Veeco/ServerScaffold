## 使用方法

* 1.git clone https://github.com/Veeco/ServerScaffold
* 2.请安装IntelliJ，用IntelliJ打开项目（由于项目比较久远，需要使用jdk1.8运行项目。另外由于本人使用的是2023的idea，所以工程适配到了gradle8.2，此版本的gradle jvm不能使用jdk 1.8，本人使用的是jdk 17）
* 3.配置好jdk目录
* 4.安装并配置mysql，确保`src/main/resources/application.properties`文件中数据与数据库配置一致（包括账号、密码、数据库名）
* 5.tools目录下有个data.sql文件，里面是sql语句，可以用navicat一键导入数据库结构
* 6.修改根目录下的gradle.properties文件，设置开发还是发版

当开发模式时，直接点击运行在浏览器输入 http://localhost:8080/home/taobaolist 即可访问。

当发版模式时，在Terminal中输入./gradlew assemble打包，在build/libs/下生成ServerScaffold-1.0.war，将这个war包拷贝到tomcat的webapps下，运行tomcat，在浏览器输入 http://localhost:8080/ServerScaffold-1.0/home/taobaolist 即可访问。

详细使用方法请参考doc目录下的[服务器搭建](https://github.com/Veeco/ServerScaffold/tree/master/doc/服务器搭建.md)。

## 目录结构

**doc目录**  一些文档说明
**tools目录** 一些工具
**src目录**
* controller 
控制器，类名用注解@RestController，里面每个方法用@RequestMapping，一个方法代表一个请求
* mapper 
数据库映射，每个方法代表一种数据库操作
* model 
实体类
* request
服务器发起http请求，用RestTemplate，这个相当于retrofit
* timer
定时任务，方法用@Scheduled代表定时
* util
工具类
* BaseApplication和ServletInitializer
启动类
* resources
资源目录
* application.properties
配置文件，目前主要配置数据库

## 程序执行顺序

作为Android程序员，我以Android的思维讲解
* 1.BaseApplication为程序入口，相当于Application
* 2.controller目录下以@RestController注解的类会自动启动，不需要手动调用，它不断监听@RequestMapping注解的方法，当客户端有请求时，即会调用,相当于Socket
* 3.mapper目录下@Select类似注解的方法会执行数据库操作，需要手动调用，完全跟sql语句一致，相当于sqlite
* 4.timer目录下以@Scheduled注解的方法会自动定时执行,不需要手动调用，相当于timer
* 5.request目录下RestTemplateController是个单例网络请求工具，需要手动调用，可以让服务器发起http请求，相当于Retrofit
