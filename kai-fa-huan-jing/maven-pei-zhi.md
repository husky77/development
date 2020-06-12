# Maven配置

#### 从开发工具集中下载解压Maven

{% page-ref page="kai-fa-gong-ju-ji.md" %}

####  **配置maven环境变量**

![](../.gitbook/assets/image%20%2810%29.png)

![](../.gitbook/assets/image%20%288%29.png)

![](../.gitbook/assets/image%20%286%29.png)

![](../.gitbook/assets/image%20%2815%29.png)

![](../.gitbook/assets/image%20%287%29.png)

 **在IntelliJ IDEA中配置maven** 

打开-File-Settings

![](../.gitbook/assets/image%20%2813%29.png)

#### 配置仓库

#### 方式1:全局配置

以添加阿里云的镜像到maven的setting.xml配置中，这样就不需要每次在pom中，添加镜像仓库的配置，在mirrors节点下面添加子节点：

```text
<id>nexus-aliyun</id>
<mirrorOf>central</mirrorOf>
<name>Nexus aliyun</name>
<url>http://maven.aliyun.com/nexus/content/groups/public</url>
```

注：&lt; mirrorOf&gt;可以设置为哪个中央仓库做镜像，为名为“central”的中央仓库做镜像，写作&lt; mirrorOf&gt;central&lt; /mirrorOf&gt;;为所有中央仓库做镜像，写作&lt; mirrorOf&gt;_&lt; /mirrorOf&gt;。Maven默认中央仓库的id 为 central。id是唯一的。 **重要：除非你有把握，否则不建议使用&lt; mirrorOf&gt;**_**&lt; /mirrorOf&gt;的方式。**

![](../.gitbook/assets/image%20%2814%29.png)

![](../.gitbook/assets/image%20%2811%29.png)

#### 方式二：单项目配置

单项目配置时，需要修改pom文件。pom文件中，没有mirror元素。在pom文件中，通过覆盖默认的中央仓库的配置，实现中央仓库地址的变更。  
修改项目的pom文件：  


```text
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>com.test</groupId>
<artifactId>conifg</artifactId>
<packaging>war</packaging>
<version>0.0.1-SNAPSHOT</version>

<repositories>
    <repository>
        <id>central</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <layout>default</layout>
        <!-- 是否开启发布版构件下载 -->
        <releases>
            <enabled>true</enabled>
        </releases>
        <!-- 是否开启快照版构件下载 -->
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```



