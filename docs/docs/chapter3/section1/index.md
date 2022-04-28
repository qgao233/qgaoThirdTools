# Maven生命周期和插件的详细介绍

https://baijiahao.baidu.com/s?id=1714329632775162386&wfr=spider&for=pc

## 1 Maven自定义属性

pom.xml中的，properties 是 project 元素的子元素，用户可以在properties 中自定义一些用户属性，然后可以在其他地方使用 ${属性名称} 这种方式进行引用。

## 2 Maven生命周期
我们开发一个项目的时候，通常有这些环节：创建项目、编写代码、清理已编译的代码、编译代码、执行单元测试、打包、集成测试、验证、部署、生成站点等，这些环节组成了项目的生命周期，这些过程也叫项目的构建过程。

maven将项目的生命周期抽象成了3套生命周期，每套生命周期又包含多个阶段，每套中具体包含哪些阶段是maven已经约定好的，但是每个阶段具体需要做什么，是用户可以自己指定的。

maven中定义的3套生命周期：

1. clean生命周期
2. default生命周期
3. site生命周期

这3套生命周期是相互独立的，没有依赖关系的，而每套生命周期中有多个阶段，每套中的多个阶段是有先后顺序的，并且后面的阶段依赖于前面的阶段，而用户可以直接使用 mvn 命令来调用这些阶段去完成项目生命周期中具体的操作，命令是：mvn 生命周期阶段。

### 2.1 clean生命周期

clean生命周期的目的是清理项目，它包含3个阶段：

1. pre-clean 执行一些需要在clean之前完成的工作

2. clean 移除所有上一次构建生成的文件

3. post-clean 执行一些需要在clean之后立刻完成的工作

我们可以通过 mvn pre-clean 来调用clean生命周期中的 pre-clean 阶段需要执行的操作。调用 mvn post-clean 会执行上面3个阶段所有的操作，每个生命周期中的后面的阶段会依赖于前面的阶段，当执行某个阶段的时候，会先执行其前面的阶段。

### 2.2 default生命周期

default生命周期是maven主要的生命周期，主要被用于构建应用，包含了23个阶段：

1. validate 校验：校验项目是否正确并且所有必要的信息可以完成项目的构建过程。

2. initialize 初始化：初始化构建状态，比如设置属性值。

3. generate-sources 生成源代码：生成包含在编译阶段中的任何源代码。

4. process-sources 处理源代码：处理源代码，比如说，过滤任意值。

5. generate-resources 生成资源文件：生成将会包含在项目包中的资源文件。

6. process-resources 编译：复制和处理资源到目标目录，为打包阶段最好准备。

7. compile 处理类文件：编译项目的源代码。

8. process-classes 处理类文件：处理编译生成的文件，比如说对Java class文件做字节码改善优化。

9. generate-test-sources 生成测试源代码：生成包含在编译阶段中的任何测试源代码。

10. process-test-sources 处理测试源代码：处理测试源代码，比如说，过滤任意值。

11. generate-test-resources 生成测试源文件：为测试创建资源文件。

12. process-test-resources 处理测试源文件：复制和处理测试资源到目标目录。

13. test-compile 编译测试源码：编译测试源代码到测试目标目录.

14. process-test-classes 处理测试类文件：处理测试源码编译生成的文件。

15. test 测试：使用合适的单元测试框架运行测试（Juint是其中之一）。

16. prepare-package 准备打包：在实际打包之前，执行任何的必要的操作为打包做准备。

17. package 打包：将编译后的代码打包成可分发格式的文件，比如JAR、WAR或者EAR文件。

18. pre-integration-test 集成测试前：在执行集成测试前进行必要的动作。比如说，搭建需要的环境。

19. integration-test 集成测试：处理和部署项目到可以运行集成测试环境中。

20. post-integration-test集成测试后：在执行集成测试完成后进行必要的动作。比如说，清理集成测试环境。

21. verify 验证：运行任意的检查来验证项目包有效且达到质量标准。

22. install 安装：安装项目包到本地仓库，这样项目包可以用作其他本地项目的依赖。

23. deploy 部署：将最终的项目包复制到远程仓库中与其他开发者和项目共享。

### 2.3 site生命周期
site生命周期的目的是建立和发布项目站点，Maven能够基于pom.xml所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。主要包含以下4个阶段：

1. pre-site 执行一些需要在生成站点文档之前完成的工作

2. site 生成项目的站点文档

3. post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备

4. site-deploy 将生成的站点文档部署到特定的服务器上。

## 3 mvn命令和生命周期
从命令行执行maven任务的最主要方式就是调用maven生命周期的阶段，需要注意的是，每套生命周期是相互独立的，但是每套生命周期中阶段是有前后依赖关系的，执行某个的时候，会按序先执行其前面所有的阶段。

mvn执行阶段的命令格式是：`mvn 阶段1 [阶段2] [阶段n]`

`mvn clean`:调用clean生命周期的clean阶段，实际执行的阶段为clean生命周期中的pre-clean和clean阶段。

`mvn test` :调用default生命周期的test阶段，实际上会从default生命周期的第一个阶段（ validate ）开始执行一直到 test 阶段结束。

`mvn clean install` ：执行了两个阶段： clean 和 install ，clean 位于 clean 生命周期中， install 位于 default 生命周期中，所以这个命令会先从 clean 生命周期中的 pre-clean 阶段开始执行一直到 clean 生命周期的 clean 阶段；然后会继续从default 生命周期的 validate 阶段开始执行一直到default生命周期的 install 阶段。

`mvn clean deploy`：先按顺序执行 clean 生命周期的 [pre-clean,clean] 这个闭区间内所有的阶段，然后按序执行 default 生命周期的 [validate,deploy] 这个闭区间内的所有阶段（也就是default 生命周期中的所有阶段）。这个命令内部包含了清理上次构建的结果、编译代码、运行单元测试、打包、将打好的包安装到本地仓库、将打好的包发布到私服仓库。

## 4 Maven插件
maven插件主要是为maven中生命周期中的阶段服务的，maven中只是定义了3套生命周期，以及每套生命周期中有哪些阶段，具体每个阶段中执行什么操作，完全是交给插件去干。

插件可以通过 mvn 命令的方式调用直接运行，或者将插件和maven生命周期的阶段进行绑定，然后通过 mvn 阶段 的方式执行阶段的时候，会自动执行和这些阶段绑定的插件。

### 4.1 插件目标
maven中的插件以jar的方式存在于仓库中，通过坐标进行访问，每个插件中可能为了代码可以重用，一个插件可能包含了多个功能。插件中的每个功能就叫做插件的目标（Plugin Goal），每个插件中可能包含一个或者多个插件目标（Plugin Goal）。

**1、目标参数**

插件目标是用来执行任务的，那么执行任务肯定是有参数配的，这些就是目标的参数，每个插件目标对应于java中的一个类(插件的类)，参数就对应于这个插件类中的属性。

**2、列出插件所有目标：**

mvn 插件goupId:插件artifactId[:插件version]:help

mvn 插件前缀:help

**3、查看插件目标参数列表**

mvn 插件goupId:插件artifactId[:插件version]:help -Dgoal=目标名称 -Ddetail

mvn 插件前缀:help -Dgoal=目标名称 -Ddetail

**4、命令行运行插件**

mvn 插件goupId:插件artifactId[:插件version]:插件目标 [-D目标参数1] [-D目标参数2] [-D目标参数n]

mvn 插件前缀:插件目标 [-D目标参数1] [-D目标参数2] [-D目标参数n]

**5、插件传参**

可以通过 `-D` 后面跟用户属性的方式给用户传参，还可以在pom.xml中

properties 的用户自定义属性中进行配置：如<maven.test.skip>true</maven.test.skip>。

**6、获取插件目标详细描述信息**

mvn help:describe -Dplugin=插件goupId:插件artifactId[:插件version] -Dgoal=目标名称 -Ddetail

mvn help:describe -Dplugin=插件前缀 -Dgoal=目标名称 -Ddetail

这个命令调用的是help插件的 describe 这个目标，这个目标可以列出其他指定插件目标的详细信息。也可以试试其他插件目标的详细信息。

### 4.2 插件前缀
运行插件的时候，可以通过指定插件坐标的方式运行。maven中给插件定义了一些简捷的插件前缀，可以通过插件前缀来运行指定的插件。

可以通过下面命令查看到插件的前缀：

mvn help:describe -Dplugin=插件goupId:插件artifactId[:插件version]

### 4.3 插件和生命周期阶段绑定

将生命周期中的阶段和插件的目标进行绑定的时候，执行 mvn 阶段 就可以执行和这些阶段绑定的 插件目标 。

### 4.4 maven内置插件以及绑定

**1、maven内置绑定**

```
clean maven-clean-plugin:clean
process-resources maven-resources-plugin:resources 复制主资源文件至主输出目录
compile maven-compiler-plugin:compile 编译主代码至主输出目录
process-test-resources maven-resources-plugin:testResources 复制测试资源文件至测试输出目录。
test-compile maven-compiler-plugin:testCompile 编译测试代码至测试输出目录
test maven-surefile-plugin:test 执行测试用例
package maven-jar-plugin:jar 创建项目jar包
install maven-install-plugin:install 将输出构件安装到本地仓库
deploy maven-deploy-plugin:deploy 将输出的构件部署到远程仓库
site maven-site-plugin:site
site-deploy maven-site-plugin:deploy
```

**2、自定义绑定**

举例：创建项目的源码jar包，将其安装到仓库中

插件 maven-source-plugin 的 jar-no-fork 可以帮助我们完成该任务，我们将这个目标绑定在default 生命周期的 verify 阶段上面。在pom.xml中配置如下：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>3.2.0</version>
            <executions>
                <!-- 使用插件需要执行的任务 -->
                <execution>
                    <!-- 任务id -->
                    <id>attach-source</id>
                    <!-- 任务中插件的目标，可以指定多个 -->
                    <goals>
                        <goal>jar-no-fork</goal>
                    </goals>
                    <!-- 绑定的阶段 -->
                    <phase>verify</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

运行命令：mvn install

## 5 POM.xml插件配置详解
### 5.1 插件目标共享参数配置

build->plugins->plugin 中配置：
```
<!-- 插件参数配置，对插件中所有的目标起效 -->
<configuration>
    <目标参数名>参数值</目标参数名>
</configuration>
```

configuration 节点下配置目标参数的值，节点名称为目标的参数名称，上面这种配置对当前插件的所有目标起效，也就是说这个插件中所有的目标共享此参数配置。

### 5.2 插件目标参数配置
project->build->plugins->plugin->executions->execution 元素中进行配置，如下：
```
<!-- 这个地方配置只对当前任务有效 -->
<configuration>
    <目标参数名>参数值</目标参数名>
</configuration>
```

这种配置常用于自定义插件绑定，只对当前任务有效。

更多maven插件的帮助文档可以参考maven的官方网站，上面有详细的介绍，建议大家去看看。

### 5.3 插件解析机制
**插件仓库配置**

插件构件也是基于坐标存储在maven仓库中。插件仓库是在pluginRepositories->pluginRepository 元素中配置的，如下：
```
<pluginRepositories>
    <pluginRepository>
        <id>myplugin-repository</id>
        <url>http://repo1.maven.org/maven2/</url>
        <releases>
            <enabled>true</enabled>
        </releases>
    </pluginRepository>
</pluginRepositories>
```

插件的默认groupId，在pom.xml中配置插件的时候，如果是官方的插件，可以省略 groupId 。如maven-compiler-plugin ，这个插件是编译代码的，是maven官方提供的插件，我们可以省略了groupId。如下：
```
<plugins>
    <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
            <compilerVersion>1.8</compilerVersion>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
    </plugin>
</plugins>
```

**插件前缀的解析**

使用mvn命令调用插件的时候，可以使用插件的前缀来代替繁琐的插件坐标的方式。

插件前缀与插件groupId:artifactId是一一对应的关系，这个关系的配置存储在仓库的元数据中，元数据位于下面2个xml中：
```
~/.m2/repository/org/apache/maven/plugins/maven-metadata-central.xml
~/.m2/repository/org/codehaus/mojo/maven-metadata-central.xml
```