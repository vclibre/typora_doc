顶级标签描述

```xml
<settings ...>
    <localRepository/>    <!--本地仓库路径-->
    <interactiveMode/>    <!--是否需要和用户交互，默认true，一般无需修改-->
    <usePluginRegistry/>  <!--是否通过pluginregistry.xml独立文件配置插件，默认false,一般直接配置到pom.xml-->
    <offline/>            <!--是否离线模式，默认false，如果不想联网，可以开启-->
    <pluginGroups/>       <!--配置如果插件groupid未提供时自动搜索，一般很少配置-->
    <servers/>            <!--配置远程仓库服务器需要的认证信息，如用户名和密码-->
    <mirrors/>            <!--为仓库列表配置镜像列表-->
    <proxies/>            <!--配置连接仓库的代理-->
    <profiles/>           <!--全局配置项目构建参数列表，一般通过它配置特定环境的定制化操作-->
    <activeProfiles/>     <!--手工激活profile，通过配置id选项完成激活-->
    <activation/>         <!--profile的扩展选项，指定某些条件下自动切换profile配置-->
    <properties/>         <!--在配置文件中声明扩展配置项-->
    <repositories/>       <!--配置远程仓库列表，用于多仓库配置-->
    <pluginRepositories/> <!--配置插件仓库列表-->
</settings>
```

完整的settings.xml配置

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- 本地仓库配置：默认~/.m2/repository[店家推荐修改配置] -->
  <localRepository>${user.home}/.m2/repository</localRepository>

  <!-- 交互方式配置，读取用户输入信息[使用默认即可，很少修改] -->
  <interactiveMode>true</interactiveMode>

  <!-- 是否启用独立的插件配置文件，一般很少启用[默认即可，很少修改] -->
  <usePluginRegistry>false</usePluginRegistry>

  <!-- 是否启用离线构建模式，一般很少修改[如果长时间不能联网的情况下可以修改] -->
  <offline>false</offline>
  
  <!-- 是否启用插件groupId自动扫描[很少使用，配置插件时建议全信息配置] -->
  <pluginGroups>
    <pluginGroup>org.apache.maven.plugins</pluginGroup>
  </pluginGroups>

  <!--配置服务端的一些设置如身份认证信息(eg: 账号、密码) -->
  <servers>
    <!--服务器元素包含配置服务器时需要的信息 -->
    <server>
      <!--这是server的id（注意不是用户登陆的id）
      该id与distributionManagement中repository元素的id相匹配。
      -->
      <id>server_001</id>
      <!--身份鉴权令牌。鉴权/认证用户名和鉴权密码表示服务器认证所需要的登录名和密码。 -->
      <username>my_login</username>
      <!--身份鉴权密码 。鉴权/认证用户名和鉴权密码表示服务器认证所需要的登录名和密码-->
      <password>my_password</password>
      <!--鉴权/认证时使用的私钥文件位置。和前两个元素类似
      私钥位置和私钥密码指定了一个私钥的路径（默认是${user.home}/.ssh/id_dsa）-->
      <privateKey>${usr.home}/.ssh/id_dsa</privateKey>
      <!--鉴权/认证时使用的私钥密码。 -->
      <passphrase>some_passphrase</passphrase>
      <!--文件被创建时的权限。如果在部署的时候会创建一个仓库文件或者目录，这时候就可以使用权限（permission）。这两个元素合法的值是一个三位数字，其对应了unix文件系统的权限，如664，或者775。 -->
      <filePermissions>664</filePermissions>
      <!--目录被创建时的权限。 -->
      <directoryPermissions>775</directoryPermissions>
    </server>
  </servers>

   <mirrors>
    <!-- 默认仓库配置给定的下载镜像位置 -->
    <mirror>
      <!-- 该镜像的唯一标识符。id用来区分不同的mirror元素。 -->
      <id>nexus aliyun</id>
      <!-- 镜像名称 -->
      <name>Nexus Aliyun</name>
      <!-- 该镜像的URL。构建系统会优先考虑使用该URL，而非使用默认的服务器URL。 -->
      <url>http://downloads.planetmirror.com/pub/maven2</url>
      <!-- 被镜像的服务器的id。
      如果我们要设置了一个Maven中央仓库（http://repo.maven.apache.org/maven2/）的镜像
      就需要将mirrorOf设置成central。
      保持和中央仓库的id central一致。 这样就能替代中央仓库的功能了-->
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>

  <proxies>
    <!--代理元素包含配置代理时需要的信息 -->
    <proxy>
      <!--代理的唯一定义符，用来区分不同的代理元素。 -->
      <id>myproxy</id>
      <!--该代理是否是激活的那个。true则激活代理。当我们声明了一组代理，而某个时候只需要激活一个代理的时候，该元素就可以派上用处。 -->
      <active>true</active>
      <!--代理的协议。 协议://主机名:端口，分隔成离散的元素以方便配置。 -->
      <protocol>http</protocol>
      <!--代理的主机名。协议://主机名:端口，分隔成离散的元素以方便配置。 -->
      <host>proxy.somewhere.com</host>
      <!--代理的端口。协议://主机名:端口，分隔成离散的元素以方便配置。 -->
      <port>8080</port>
      <!--代理的用户名，用户名和密码表示代理服务器认证的登录名和密码。 -->
      <username>proxyuser</username>
      <!--代理的密码，用户名和密码表示代理服务器认证的登录名和密码。 -->
      <password>somepassword</password>
      <!--不该被代理的主机名列表。该列表的分隔符由代理服务器指定；例子中使用了竖线分隔符，使用逗号分隔也很常见。 -->
      <nonProxyHosts>*.google.com|ibiblio.org</nonProxyHosts>
    </proxy>
  </proxies>

  <profiles>
    <profile>
      <!-- profile的唯一标识 -->
      <id>test</id>
      <!-- 自动触发profile的条件逻辑 -->
      <activation />
      <!-- 扩展属性列表 -->
      <properties />
      <!-- 远程仓库列表 -->
      <repositories />
      <!-- 插件仓库列表 -->
      <pluginRepositories />
    </profile>
  </profiles>


  <activeProfiles>
    <!-- 要激活的profile id -->
    <activeProfile>env-test</activeProfile>
  </activeProfiles>

  <activation>
    <!--profile默认是否激活的标识 -->
    <activeByDefault>false</activeByDefault>
    <!--当匹配的jdk被检测到，profile被激活。例如，1.4激活JDK1.4，1.4.0_2，而!1.4激活所有版本不是以1.4开头的JDK。 -->
    <jdk>1.5</jdk>
    <!--当匹配的操作系统属性被检测到，profile被激活。os元素可以定义一些操作系统相关的属性。 -->
    <os>
      <!--激活profile的操作系统的名字 -->
      <name>Windows XP</name>
      <!--激活profile的操作系统所属家族(如 'windows') -->
      <family>Windows</family>
      <!--激活profile的操作系统体系结构 -->
      <arch>x86</arch>
      <!--激活profile的操作系统版本 -->
      <version>5.1.2600</version>
    </os>
    <!--如果Maven检测到某一个属性（其值可以在POM中通过${name}引用），其拥有对应的name = 值，Profile就会被激活。如果值字段是空的，那么存在属性名称字段就会激活profile，否则按区分大小写方式匹配属性值字段 -->
    <property>
      <!--激活profile的属性的名称 -->
      <name>mavenVersion</name>
      <!--激活profile的属性的值 -->
      <value>2.0.3</value>
    </property>
    <!--提供一个文件名，通过检测该文件的存在或不存在来激活profile。missing检查文件是否存在，如果不存在则激活profile。另一方面，exists则会检查文件是否存在，如果存在则激活profile。 -->
    <file>
      <!--如果指定的文件存在，则激活profile。 -->
      <exists>${basedir}/file2.properties</exists>
      <!--如果指定的文件不存在，则激活profile。 -->
      <missing>${basedir}/file1.properties</missing>
    </file>
  </activation>

<properties>
  <spring.Version>5.2.8</spring.Version>
</properties>

<repositories>
  <!--包含需要连接到远程仓库的信息 -->
  <repository>
    <!--远程仓库唯一标识 -->
    <id>codehausSnapshots</id>
    <!--远程仓库名称 -->
    <name>Codehaus Snapshots</name>
    <!--如何处理远程仓库里发布版本的下载 -->
    <releases>
      <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->
      <enabled>false</enabled>
      <!--该元素指定更新发生的频率。Maven会比较本地POM和远程POM的时间戳。这里的选项是：always（一直），daily（默认，每日），interval：X（这里X是以分钟为单位的时间间隔），或者never（从不）。 -->
      <updatePolicy>always</updatePolicy>
      <!--当Maven验证构件校验文件失败时该怎么做-ignore（忽略），fail（失败），或者warn（警告）。 -->
      <checksumPolicy>warn</checksumPolicy>
    </releases>
    <!--如何处理远程仓库里快照版本的下载。有了releases和snapshots这两组配置，POM就可以在每个单独的仓库中，为每种类型的构件采取不同的策略。例如，可能有人会决定只为开发目的开启对快照版本下载的支持。参见repositories/repository/releases元素 -->
    <snapshots>
      <enabled />
      <updatePolicy />
      <checksumPolicy />
    </snapshots>
    <!--远程仓库URL，按protocol://hostname/path形式 -->
    <url>http://snapshots.maven.codehaus.org/maven2</url>
    <!--用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。 -->
    <layout>default</layout>
  </repository>
</repositories>

</settings>
```

