# my-action-maven

此处列出我关于 Gradle 的学习实践。


# 资源

- [maven resources inheritance problem](http://www.4e00.com/blog/java/2016/06/09/maven-resources-inheritance-problem.html)

# HandBook

## 一、常见结构

```xml
<project>
    <modelVersion></modelVersion>
    <modules>
        <module></module>
    </modules>

    <parent>
        <artifactId></artifactId>
        <groupId></groupId>
        <version></version>
    </parent>

    <groupId></groupId>
    <artifactId></artifactId>
    <version></version>
    <packaging></packaging>

    <properties>
        <XXX.version></XXX.version>
        ...
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId></groupId>
                <artifactId></artifactId>
                <version>${XXX.version}</version>

                <exclusions>
                    <exclusion>
                        <groupId></groupId>
                        <artifactId></artifactId>
                    </exclusion>
                    ...
                </exclusions>
            </dependency>
        </dependencies>
        ...
    </dependencyManagement>

    <!-- common dependencies -->
    <dependencies>
        <dependency>
            ...
            <scope>test</scope>
        </dependency>
        ...
    </dependencies>

    <!-- Profiles settings -->
    <profiles>
        <profile>
            <id>local</id>
            <properties>
                <XXX.env>local</XXX.env>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        ...
        <profile>
            <id>release</id>
            <properties>
                <XXX.env>release</XXX.env>
            </properties>
        </profile>
    </profiles>

    <!-- Build settings -->
    <build>
        <resources>
            <resource>
                <directory>...</directory>
                <includes>
                    <include>*</include>
                </includes>
            </resource>
            <resource>
                <directory>...</directory>
                <excludes>
                    <exclude>../**</exclude>
                </excludes>
            </resource>
        </resources>

        <pluginManagement>
            <plugins>
                <plugin>
                    <artifactId></artifactId>
                    <version></version>
                    <executions>
                        <execution>
                            <id></id>
                            <phase></phase>
                            <goals>
                                <goal></goal>
                            </goals>
                            <configuration>
                                ...
                            </configuration>
                        </execution>
                    </executions>
                </plugin>
                ...
            </plugins>
        </pluginManagement>

        <plugins>
            <plugin>
                <groupId></groupId>
                <artifactId></artifactId>
                <version></version>
                <configuration>
                    ...
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId></groupId>
                        <artifactId></artifactId>
                        <version></version>
                    </dependency>
                </dependencies>
            </plugin>
            ...
        </plugins>
    </build>

    <repositories>
        <repository>
            <id></id>
            <url></url>
        </repository>
        ...
    </repositories>

    <!-- other settings, less use -->

    <!-- project information -->
    <name></name>
    <description></description>
    <url></url>
    <inceptionYear></inceptionYear>
    <licenses></licenses>
    <organization></organization>
    <developers></developers>
    <contributors></contributors>

    <!-- env settings -->
    <issueManagement></issueManagement>
    <ciManagement></ciManagement>
    <mailingLists></mailingLists>
    <scm></scm>
    <prerequisites></prerequisites>
    <pluginRepositories></pluginRepositories>
    <distributionManagement></distributionManagement>

    <!-- reporting -->
    <reporting>
        <excludeDefaults></excludeDefaults>
        <outputDirectory></outputDirectory>
        <plugins>
            <plugin>
                <artifactId></artifactId>
                <version></version>
                <reportSets>
                    <reportSet>
                        <id></id>
                        <reports>
                            <report></report>
                        </reports>
                        <inherited></inherited>
                        <configuration>
                        <links>
                            <link></link>
                        </links>
                        </configuration>
                    </reportSet>
                </reportSets>
            </plugin>
        </plugins>
    </reporting>

</project>

```

## 二、 常用 Profiles

- local
- alpha
- beta
- rc
- release

## 三、 常用 Plugins

- maven-assembly-plugin
- maven-eclipse-plugin
- maven-compiler-plugin
- maven-shade-plugin
- maven-source-plugin
- maven-surefire-plugin
- jacoco-maven-plugin
- git-commit-id-plugin
- exec-maven-plugin
- spring-boot-maven-plugin
- http://maven.apache.org/plugins/index.html

## 四、 常用隐式变量

- ${env.PATH}:获取环境变量path的值
- ${basedir} 项目根目录
- ${project.groudId}
- ${project.artifactId}
- ${project.version} 项目版本号与 ${version} 等价
- ${project.build.sourceDirectory} 项目主源码目录，默认为 src/main/java
- ${project.build.testSourceDirectory} 项目测试源码目录，默认为 /src/test/java/
- ${project.build.directory} 构建目录，缺省为target/
- ${project.build.outputDirectory} 构建过程输出目录，缺省为target/classes/
- ${project.build.testOputDirectory} 构建过程输出目录，缺省为target/test-classes/
- ${project.build.finalName} 产出物名称，缺省为${project.artifactId}-${project.version}
- ${project.packaging} 打包类型，缺省为jar
- ${project.xxx} 当前pom文件的任意节点的内容
- ${settings.offline} 引用~/.m2/settings.xml文件中offline元素的值

## 五、 <packaging>取值范围
- jar (默认值)
- pom
- war
- ear
- rar
- par
- ejb
- maven-plugin

## 六、 <scope>取值范围
- compile (默认值)
- test
- provided
- runtime
- system
- import

## 七、 环境变量与配置文件
- M3_HOME 
- MAVEN_OPTS -Xms128m -Xms512m
- M3_HOME/conf/settings.xml
- ~/.m2/settings.mxl

## 八、 常用命令
- mvn clean compile
- mvn clean test
- mvn clean install
- mvn clean deploy
- mvn dependency:list
- mvn dependency:tree
- mvn dependency:analyze

- 查看Plugin信息: `mvn help:describe-Dplugin=org.apache.maven.plugins:maven-source-plugin:2.1.1-Ddetail`  
- 命令行插件配置: `mvn install -Dmaven.test.skip=true`
- mvn clean install -Pdev
- mvn help:active-profiles
- mvn help:all-profiles
- mvn site:stage -DstagingDirectory=D:\tmp
- mvn archetype:generate


非交互式创建 maven-archetype:
```
$ > mvn archetype:generate -B \
-DarchetypeGroupId = org.apache.maven.archetypes \
-DarchetypeArtifactId = maven-archetype-quickstart \
-DarchetypeVersion = 1.0 \
-DgroupId = com.wjpdev.maven \
-Dartifactid = maven-demo-test \
-Dversion = 1.0-SNAPSHOT \ 
-Dpackage = com.wjpdev.maven
```

## 九、 Maven三套生命周期

#### Clean 生命周期
- pre-clean
- **clean**
- post-clean

#### default 生命周期
- validate
- initialize
- generate-sources
- process-sources
- **compile**
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- **test**
- prepare-package
- **package**
- pre-integration-test
- integration-test
- post-integration-test
- verify
- **install**
- **deploy**

#### site 生命周期
- pre-site
- site
- post-site
- site-deploy

## 十、 41个 Archetype:

1. appfuse-basic-jsf (创建一个基于Hibernate，Spring和JSF的Web应用程序的原型) 
2. appfuse-basic-spring(创建一个基于Hibernate，Spring和Spring MVC的Web应用程序的原型) 
3. appfuse-basic-struts(创建一个基于Hibernate，Spring和Struts 2的Web应用程序的原型) 
4. appfuse-basic-tapestry(创建一个基于Hibernate，Spring 和 Tapestry 4的Web应用程序的原型) 
5. appfuse-core(创建一个基于Hibernate，Spring 和 XFire的jar应用程序的原型) 
6. appfuse-modular-jsf(创建一个基于Hibernate，Spring和JSF的模块化应用原型) 
7. appfuse-modular-spring(创建一个基于Hibernate, Spring 和 Spring MVC 的模块化应用原型) 
8. appfuse-modular-struts(创建一个基于Hibernate, Spring 和 Struts 2 的模块化应用原型) 
9. appfuse-modular-tapestry (创建一个基于 Hibernate, Spring 和 Tapestry 4 的模块化应用原型) 
10. maven-archetype-j2ee-simple(一个简单的J2EE的Java应用程序) 
11. maven-archetype-marmalade-mojo(一个Maven的 插件开发项目 using marmalade) 
12. maven-archetype-mojo(一个Maven的Java插件开发项目) 
13. maven-archetype-portlet(一个简单的portlet应用程序) 
14. maven-archetype-profiles() 
15. maven-archetype-quickstart() 
16. maven-archetype-site-simple(简单的网站生成项目) 
17. maven-archetype-site(更复杂的网站项目) 
18. maven-archetype-webapp(一个简单的Java Web应用程序) 
19. jini-service-archetype(Archetype for Jini service project creation) 
20. softeu-archetype-seam(JSF+Facelets+Seam Archetype) 
21. softeu-archetype-seam-simple(JSF+Facelets+Seam (无残留) 原型) 
22. softeu-archetype-jsf(JSF+Facelets 原型) 
23. jpa-maven-archetype(JPA 应用程序) 
24. spring-osgi-bundle-archetype(Spring-OSGi 原型) 
25. confluence-plugin-archetype(Atlassian 聚合插件原型) 
26. jira-plugin-archetype(Atlassian JIRA 插件原型) 
27. maven-archetype-har(Hibernate 存档) 
28. maven-archetype-sar(JBoss 服务存档) 
29. wicket-archetype-quickstart(一个简单的Apache Wicket的项目) 
30. scala-archetype-simple(一个简单的scala的项目) 
31. lift-archetype-blank(一个 blank/empty liftweb 项目) 
32. lift-archetype-basic(基本（liftweb）项目) 
33. cocoon-22-archetype-block-plain([http://cocoapacorg2/maven-plugins/]) 
34. cocoon-22-archetype-block([http://cocoapacorg2/maven-plugins/]) 
35. cocoon-22-archetype-webapp([http://cocoapacorg2/maven-plugins/]) 
36. myfaces-archetype-helloworld(使用MyFaces的一个简单的原型) 
37. myfaces-archetype-helloworld-facelets(一个使用MyFaces和Facelets的简单原型) 
38. myfaces-archetype-trinidad(一个使用MyFaces和Trinidad的简单原型) 
39. myfaces-archetype-jsfcomponents(一种使用MyFaces创建定制JSF组件的简单的原型) 
40. gmaven-archetype-basic(Groovy的基本原型) 
41. gmaven-archetype-mojo(Groovy mojo 原型)



