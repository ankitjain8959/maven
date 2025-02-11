# What is Maven?
Maven is a build automation tool primarily used for Java projects.
It simplifies the build process by managing project dependencies, compiling source code, packaging the compiled code into JAR files, and deploying the artifacts to repositories.

```
https://maven.apache.org/guides/introduction/introduction-to-the-pom.html
```

# pom.xml file
pom stands for Project Object Model.<br>
The pom.xml file is the core of a Maven project. It contains information about the project and configuration details used by Maven to build the project.

# Key concepts and features of Maven
- dependencies <br>
  Maven manages project dependencies, which are external libraries required by the project.
  Dependencies are specified in the pom.xml file, and Maven automatically downloads them from a central repository.
  ```
  <dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.5.4</version>
    </dependency>
  </dependencies> 
  ```
- repositories <br>
  Maven uses repositories to store and retrieve dependencies. <br>
  There are three types of repositories: <br>
  Local Repository: A directory on the developer's machine where Maven stores downloaded artifacts.
  Central Repository: A public repository provided by the Maven community.
  Remote Repository: Custom repositories defined by the user or organization.
  ```
  <repositories>
    <repository>
        <id>Digital</id>
        <url>https://pkgs.dev.azure.com/digital-org/_packaging/Digital/maven/v1</url>
        <releases>
           <enabled>true</enabled>
        </releases>
        <snapshots>
           <enabled>true</enabled>
        </snapshots>
    </repository>
  </repositories>
  ```

- plugins <br>
  Maven uses plugins to perform tasks during the build process. <br>
  Plugins are specified in the pom.xml file and can be used for compiling code, running tests, generating documentation, etc.
  ```
  <build>
    <plugins>
      <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-compiler-plugin</artifactId>
         <version>3.8.1</version>
         <configuration>
             <source>11</source>
             <target>11</target>
         </configuration>
      </plugin>
    </plugins>
    </build>
    ```

# Minimal pom.xml file:
```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
</project>
```
The minimum requirement for a POM are the following:

- project: (Mandatory element) root tag for the POM file
- modelVersion: (Mandatory element) specifies the version of the Project Object Model (POM) that the project is using. It should be set to 4.0.0
- groupId: the id of the project's group.
- artifactId: the id of the artifact (project)
- version: the version of the artifact under the specified group

The `<groupId>`, `<artifactId>`, and `<version>` elements uniquely identify the project and can be referred to as the Project Coordinates. <br>
These three values form the project's fully qualified artifact name. This is in the form of `<groupId>`:`<artifactId>`:`<version>`.
As for the example above, its fully qualified artifact name is "com.mycompany.app:my-app:1".

If some of the configuration details are not specified (example: packaging, repositories), Maven will use their defaults. <br>
The `Super POM` is Maven's default POM. All POMs extend the Super POM unless explicitly set. <br>
```
https://maven.apache.org/ref/3.9.9/maven-model-builder/super-pom.html
```
It is located in the maven-model-builder JAR file within the Maven installation directory.
The exact location of the Super POM on a local developer's machine is typically:
```
$MAVEN_HOME/lib/maven-model-builder-<version>.jar
```
Inside this JAR file, the Super POM is located at:
```
org/apache/maven/model/pom-4.0.0.xml
```

- `packaging`: Every Maven project has a packaging type. If it is not specified in the POM, then the default value "jar" would be used.
- `repositories`: If you build your project using the minimal POM, it would inherit the repositories configuration in the Super POM. Therefore when Maven sees the dependencies in the minimal POM, it would know that these dependencies will be downloaded from https://repo.maven.apache.org/maven2 which was specified in the Super POM


