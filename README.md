1. maven repositoy github export 사용방법

pom.xml 설정

```xml

<groupId>org.example</groupId>
<artifactId>core</artifactId>
<version>1.1</version>

<properties>
	<github.global.server>github</github.global.server>
</properties>

<distributionManagement>
    <repository>
        <id>internal.repo</id>
        <name>Temporary Staging Repository</name>
        <url>file://${project.build.directory}/mvn-repo</url>
    </repository>
</distributionManagement>

<build>
	<plugins>
		<plugin>
      <artifactId>maven-deploy-plugin</artifactId>
      <version>2.8.2</version>
      <configuration>
          <altDeploymentRepository>internal.repo::default::file://${project.build.directory}/mvn-repo
          </altDeploymentRepository>
      </configuration>
	  </plugin>

		<plugin>
      <groupId>com.github.github</groupId>
      <artifactId>site-maven-plugin</artifactId>
      <version>0.12</version>
      <configuration>
          <message>Maven artifacts for ${project.version}</message>  <!-- git commit message -->
          <noJekyll>true</noJekyll>                                  <!-- disable webpage processing -->
          <outputDirectory>${project.build.directory}/mvn-repo</outputDirectory> <!-- matches distribution management repository url above -->
          <branch>refs/heads/core</branch>                    <!-- github remote branch name -->
          <includes><include>**/*</include></includes>
          <repositoryName>maven</repositoryName>              <!-- github repository name -->
          <repositoryOwner>Kwakjeonghwan</repositoryOwner>    <!-- github username  -->
      </configuration>
      <executions>
          <execution>
              <goals>
                  <goal>site</goal>
              </goals>
              <phase>site</phase>
          </execution>
      </executions>
	  </plugin>
	</plugins>
</build>
```

settings.xml ( maven 설정 파일 )

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <servers>
    <server>
      <id>github</id>
			<!-- github access_token -->
      <password>xxx4d4f4766368ec9afa9bc28146954daf91fd7f</password>
    </server>
  </servers>
</settings>
```

- maven repository 파일 생생

    mvn clean deploy

- maven repository 생성 파일 github에 upload

    mvn site:site

2. github maven repository import 사용방법

pom.xml

```xml
<properties>
	<github.global.server>github</github.global.server>
</properties>

<repositories>
	<repository>
    <id>core</id>
    <url>https://raw.github.com/Kwakjeonghwan/maven/core01</url>
    <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
    </snapshots>
  </repository>
<repositories>

<dependencies>
  <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.7</version>
  </dependency>
</dependencies>
```

settings.xml ( maven 설정 파일 )

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>github</id>
      <password>2f34d4f4766368ec9afa9bc28146954daf91fd7f</password>
      <configuration>
        <httpHeaders>
          <!-- Base64-encoded username:access_token -->
          <authorization>Basic xxxha2plb25naHdhbjoyZjM0ZDRmNDc2NjM2OGVjOWFmYTliYzI4MTQ2OTU0ZGFmOTFmZDdm</authorization>
        </httpHeaders>
      </configuration>
    </server>
  </servers>

</settings>
```
