## Maven安装

mavan安装比较简单，只需要在官网下载mavan.zip包后配置好环境变量就行，后在命令行中mvn -v,显示出版本信息后则为配置成功，后在idea中导入mavan。

### 在idea中配置mavan

1. 打开idea
2. file-setting-build-build tool-mavan后进行配置。
3. 如果没有build这一选项，可在edited中的plugin中搜索mavan后勾选重启idea即可


## Maven常用插件简单配置

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  
  <modelVersion>4.0.0</modelVersion>
  
  <!-- 坐标、版本以及打包方式 -->
  <groupId>com.alanlee</groupId>
  <artifactId>UidpWeb</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  
  <!-- maven属性的使用 -->
  <properties>
      <plugin.version>2.5</plugin.version>
  </properties>
  
  <!-- 依赖配置的使用 -->
  <dependencies>
      
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <!-- 测试范围有效，在编译和打包时都不会使用这个依赖 -->
        <scope>test</scope>
    </dependency>
    
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <!-- 在编译和测试的过程有效，最后生成war包时不会加入 -->
        <scope>provided</scope>
    </dependency>
    
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <!-- 在编译和测试的过程有效，最后生成war包时不会加入 -->
        <scope>provided</scope>
    </dependency>
    
  </dependencies>
  
  <!-- 用来支持项目发布到私服中,用来配合deploy插件的使用 -->
  <distributionManagement>
      <!-- 发布版本 -->
    <repository>
        <id>releases</id>
        <name>public</name>
        <url>http://10.200.11.21:8081/nexus/content/repositories/releases/</url>
    </repository>
    <!-- 快照版本 -->
    <snapshotRepository>
        <id>snapshots</id>
        <name>Snapshots</name>
        <url>http://10.200.11.21:8081/nexus/content/repositories/snapshots</url>
    </snapshotRepository>
  </distributionManagement>
  
  <!-- 注意体会插件配置的顺序，这正体现了一个maven的运行流程 -->
  <build>
      <plugins>
          <!-- 插件使用练习 -->
          <!-- 清理插件的使用，maven3.0.4会默认使用2.4.1版本的clean插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-clean-plugin</artifactId>
            <version>${plugin.version}</version>
            <executions>
                <execution>
                    <id>auto-clean</id>
                    <!-- clean生命周期clean阶段 -->
                    <phase>clean</phase>
                    <goals>
                        <!-- 执行clean插件的clean目标 -->
                        <goal>clean</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        
        <!-- maven-resources-plugin在maven3.0.4中默认使用2.5版本的resources -->
        
        <!-- 编译插件的使用，maven3.0.4会默认使用2.3.2版本的compile插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>${plugin.version}</version>
            <configuration>
                <!-- 源代码使用的jdk版本 -->
                <source>1.7</source>
                <!-- 构建后生成class文件jdk版本 -->
                <target>1.7</target>
            </configuration>
        </plugin>
        
        <!-- maven-surefire-plugin插件，maven3.0.4默认使用2.10版本的surefire插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>${plugin.version}</version>
            <configuration>
                <!-- 改变测试报告生成目录 ，默认为target/surefire-reports-->
                <!-- project.build.directory表示maven的属性，这里指的是构建的目录下面test-reports，project.build.directory就是pom标签的值 -->
                <reportsDirectory>${project.build.directory}/test-reports</reportsDirectory>
            </configuration>
        </plugin>
        
        <!-- war包插件的使用，maven3.0.4会默认使用xxx版本的war插件，建议配置编码格式和打包名称 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <!-- 利用属性传递版本号 -->
            <version>${plugin.version}</version>
            <configuration>
                <!-- 设置编码 -->
                <encoding>UTF-8</encoding>
                <!-- 设置名称 -->
                <warName>ROOT</warName>
            </configuration>
        </plugin>
        
        <!-- maven-install-plugin插件一般不需要配置,maven3.0.4默认使用2.3.1版本的install插件 -->
        
        <!-- 部署插件的使用，maven3.0.4会默认使用2.7版本的deploy插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-deploy-plugin</artifactId>
            <version>${plugin.version}</version>
            <configuration>
                <!-- 更新元数据 -->
                <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
        </plugin>
        
    </plugins>
  </build>
  
</project>
```