# JavaWeb系列 5 maven
#javaweb
## maven的命令
* clean  删除构建目录
* validate 检查工程配置是否正确
* compile 编译
* test 使用适当测试框架测试
* package 获取编译后的代码，并按照可发布的格式进行打包
* verity  验证打的包是否是正确的
* install   安装工程包到本地仓库中，该仓库可以作为本地其他工程的依赖。
* site   生成项目的帮助文档
* deploy 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程。

常用：   clean   compile   package   install   deploy


## 仓库
Maven 依赖搜索顺序

1. － 在本地仓库中搜索
2. － 在中央仓库中搜索
3. 查看是否存在一个或多个远程仓库已经设置，设置如下：
4. － 在一个或多个远程仓库中搜索依赖的文件

pom.xml中查找仓库

> <repositories>
>      <repository>
>         <id>companyname.lib1</id>
>         <url>http://download.companyname.org/maven2/lib1</url>
>      </repository>
>      <repository>
>         <id>companyname.lib2</id>
>         <url>http://download.companyname.org/maven2/lib2</url>
>      </repository>
>   </repositories>

## 配置文件   profile
> <profiles>
>    <profile>
>        <id>dev</id>
>        <properties>
>            <profiles.active>develop</profiles.active>
>        </properties>
>    </profile>
> </profiles> 

可将配置项填入变量中
即  dev环境下     profiles.active = develop





