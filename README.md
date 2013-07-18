mvn-repo
========

source : http://stackoverflow.com/questions/14013644/hosting-a-maven-repository-on-github

* pom.xml
```xml
<properties>
        <github.global.server>github</github.global.server>
</properties>
```

```xml
 <plugin>
    <artifactId>maven-deploy-plugin</artifactId>
    <version>2.7</version>
    <configuration>
        <altDeploymentRepository>internal.repo::default::file://${project.build.directory}/mvn-repo</altDeploymentRepository>
    </configuration>
</plugin>
<plugin>
    <groupId>com.github.github</groupId>
    <artifactId>site-maven-plugin</artifactId>
    <version>0.8</version>
    <configuration>
        <message>Maven artifacts for ${project.version}</message>  <!-- git commit message -->
        <noJekyll>true</noJekyll>                                  <!-- disable webpage processing -->
        <outputDirectory>${project.build.directory}/mvn-repo</outputDirectory> <!-- matches distribution management repository url above -->
        <branch>refs/heads/master</branch>                       <!-- remote branch name -->
        <includes><include>**/*</include></includes>
        <repositoryName>mvn-repo</repositoryName>      <!-- github repo name -->
        <repositoryOwner>tbruyelle</repositoryOwner>    <!-- github username  -->
        <merge>true</merge>
    </configuration>
    <executions>
        <!-- run site-maven-plugin's 'site' target as part of the build's normal 'deploy' phase -->
        <execution>
            <goals>
                <goal>site</goal>
            </goals>
            <phase>deploy</phase>
        </execution>
    </executions>
</plugin>
```            

```xml
<distributionManagement>
    <repository>
        <id>github</id>
        <name>GitHub Repository</name>
        <url>https://raw.github.com/tbruyelle/mvn-repo/master</url>
    </repository>
</distributionManagement>
```

* settings.xml
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
        http://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
                <server>
                        <id>github</id>
                        <username>tbruyelle</username>
                        <password>...</password>
                </server>
        </servers>
</settings>
```
  
