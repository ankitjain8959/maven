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
    - Local Repository: A directory on the developer's machine where Maven stores downloaded artifacts.  
    - Remote Repository: Custom repositories defined by the user or organization.  
    - Central Repository: A public repository provided by the Maven community.  
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
- `repositories`: If you build your project using the minimal POM, it would inherit the repositories configuration in the Super POM. Therefore, when Maven sees the dependencies in the minimal POM, it would know that these dependencies will be downloaded from https://repo.maven.apache.org/maven2 which was specified in the Super POM.


# Maven Lifecycle
A Lifecycle in Maven is a sequence of phases (i.e. steps) that Maven executes during the build process.
Each phase represents a specific step in the process of building, testing, and deploying a project.

## Maven defines three main lifecycles:
- Default Lifecycle (for building the project)
- Clean Lifecycle (for cleaning the project)
- Site Lifecycle (for generating and deploying project documentation)


### Default Lifecycle
The Default Lifecycle is the most important lifecycle in Maven. <br>
It is responsible for building the project and creating artifacts like JAR, WAR, and EAR files. <br>
The Default Lifecycle consists of the following phases: <br>
`validate`	- Validates the project structure and pom.xml file. <br>
`compile`	- Compiles the source code (src/main/java). <br>
`test`	- Runs tests (src/test/java). <br>
`package`	- Packages the code into a distributable format (e.g., JAR, WAR). <br>
`verify`	- Performs any additional checks (like running integration tests) on the project. <br>
`install`	- Installs the packaged artifact into the local repository (~/.m2/repository). <br>
`deploy`	- Deploys the artifact to a remote repository (e.g., Nexus, Artifactory). <br>


### Clean Lifecycle
The Clean Lifecycle is responsible for cleaning the project. It removes all files generated by the previous build. <br>
The Clean Lifecycle consists of the following phases: <br>
`pre-clean`	- Executes tasks before the project is cleaned. (optional) <br>
`clean`	- Removes all files generated by the previous build. Deletes the /target directory. <br>
`post-clean`	- Executes tasks after the project is cleaned. (optional) <br>


### Site Lifecycle
The Site Lifecycle is responsible for generating project documentation & reports and deploying it to a web server or repository <br>
The Site Lifecycle consists of the following phases: <br>
`pre-site`	- Executes tasks before the site is generated. (optional) <br>
`site`	- Generates the project site (i.e. documentation & reports) <br>
`post-site`	- Executes tasks after the site is generated. (optional) <br>
`site-deploy`	- Deploys the generated documentation to a web server or repository <br>


### Running a Lifecycle
To run a lifecycle, you can use the following command: <br>
```
mvn <phase>
```
Example: <br>
```
mvn compile
```
This command will execute the compile phase of the Default Lifecycle. <br>
If you run a phase, Maven will execute all preceding phases in the lifecycle. <br>
For example,
- running the `clean phase` will execute pre-clean and clean phases in order.
- running the `compile phase` will execute validate and compile phases in order.
- running the `package phase` will execute validate, compile, test, and package phases in order.
- running the `install phase` will execute validate, compile, test, package, verify, and install phases in order.
- running the `deploy phase` will execute validate, compile, test, package, verify, install, and deploy phases in order.
- running the `verify phase` will execute validate, compile, test, package, and verify phases in order.
- running the `test phase` will execute validate, compile, and test phases in order.
- running the `validate phase` will execute only the validate phase.


# Packaging
1. packaging value is set using the `<packaging>` tag in the POM file. <br>
2. packaging value is used to specify the type of artifact that the project should produce. <br>
3. Some of the valid packaging values are jar, war, ear and pom. <br>
4. If no packaging value has been specified, it will default to jar. <br>

# Dependency Management in Maven
Maven’s `<dependencyManagement>` ensures consistency across projects by defining dependency versions in a centralized way. The <dependencyManagement> section is mainly used in multi-module projects where a parent POM defines dependency versions, and child modules inherit them without explicitly specifying versions.

## Why Use `<dependencyManagement>`?
- Avoid Version Conflicts: All modules in a project use the same dependency versions.
- Better Maintainability: Versions are managed in one place (parent POM), making upgrades easier.
- Avoid Repetition: Child modules declare dependencies without specifying versions.
- Prevent Implicit Upgrades: Without <dependencyManagement>, transitive dependencies might introduce unexpected versions.

### Transitive Dependencies:
When you add a dependency in Maven, it might bring other dependencies along called transitive dependencies.
If you don’t control versions explicitly, Maven may choose a different version than expected, leading to unexpected behavior or compatibility issues.

### Example
Let’s assume we have a multi-module Maven project:<br>

- A parent POM (which manages versions).
- Two child modules:
  - web-service: Uses Spring Boot Web.
  - database-service: Uses Spring Boot Data JPA and PostgreSQL.

1. Parent POM (pom.xml in the root project)
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>parent-project</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <modules>
        <module>web-service</module>
        <module>database-service</module>
    </modules>

    <dependencyManagement>
        <dependencies>
            <!-- Spring Boot Web Starter -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>3.2.0</version>
            </dependency>

            <!-- Spring Boot Data JPA -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-jpa</artifactId>
                <version>3.2.0</version>
            </dependency>

            <!-- PostgreSQL Driver -->
            <dependency>
                <groupId>org.postgresql</groupId>
                <artifactId>postgresql</artifactId>
                <version>42.6.0</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

2. Child Module: web-service (pom.xml in web-service module)
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.example</groupId>
        <artifactId>parent-project</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>web-service</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```
✅ No version specified for spring-boot-starter-web—it is inherited from dependencyManagement.

3. Child Module: database-service (pom.xml in database-service module)
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.example</groupId>
        <artifactId>parent-project</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>database-service</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
        </dependency>
    </dependencies>

</project>
```
✅ No version specified for spring-boot-starter-data-jpa or postgresql—they are inherited from dependencyManagement.


### How This Helps in a Real Project?
If a newer version of Spring Boot (e.g., 3.2.1) is released, you only update the parent POM:
```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>3.2.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
            <version>3.2.1</version>
        </dependency>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.6.1</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```
Now, all child modules automatically use these new versions without modifying their POM files.


### Key Takeaways
✅ Without `<dependencyManagement>`:
Each child module would need to specify versions manually, leading to inconsistencies.
Upgrading dependencies would require changes in all child modules.
Transitive dependencies might introduce unexpected versions.

✅ With `<dependencyManagement>`:
All child modules inherit versions automatically.
Easier upgrades — only the parent POM needs changes.
Consistency — ensures all modules use the same dependency versions.

## Using BOM (Bill of Materials)
Maven POM is an XML file that contains information and configurations (about the project) that are used by Maven to import dependencies and build the project.<br>
BOM stands for Bill Of Materials (detailed list of all the parts, materials, and components needed to build a product.  It includes what items are required, their quantities, and their specifications).<br>
A BOM is a special kind of POM that is used to control the versions of a project’s dependencies and provide a central place to define and update those versions.

### Why Use BOM?
For well-maintained frameworks like Spring Boot, a better approach is to import their BOM instead of managing individual dependency versions.
Spring Boot provides a BOM that manages versions of all Spring dependencies. By importing this BOM, you can avoid specifying versions for each dependency.

When using <dependencyManagement>, we can import a BOM, which is essentially a POM file that defines versions for multiple dependencies. This is done using:

`<type>pom</type>`
Normally, when you add dependencies, Maven assumes JAR as the default type.
However, BOM is a POM file, not a JAR, so we must explicitly declare it as `<type>pom</type>`

`<scope>import</scope>`
It ensures that the dependency versions defined in the BOM are applied globally to your project.<br>

Note: The above 2 tags tells Maven that we are not including an actual dependency but rather importing version information from another POM.

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>3.2.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

✅ No need to manually specify versions for Spring Boot dependencies—they are inherited from the BOM.

After Importing BOM, Child Modules Can Omit Versions:
```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
</dependencies>
```


# Build & Package the Project
## Clean
Remove old Build (.class) files
```
mvn clean
```
✅ Deletes the target/ directory (removes compiled code and artifacts).
✅ Use this before building the project to ensure a fresh build.


## Compile
Compile Source Code
```
mvn compile
```
✅ Compiles the project’s Java source code (src/main/java).
✅ Does not run tests.

## Package
Create JAR/WAR File
```
mvn package
```
✅ Compiles the code and packages it into a JAR/WAR file inside the target/ folder.
✅ Example: target/my-app-1.0.jar

## Install
Install JAR in Local Repository
```
mvn install
```
✅ Builds the project and stores the generated JAR/WAR in the local Maven repository (~/.m2/repository).
✅ This allows other projects on the same machine to use this package as a dependency.

## Deploy
- Deploy artifacts (JAR/WAR) to Remote Repository<br>
```
mvn deploy
```
✅ When you have a full Maven project (pom.xml) and want to deploy its generated artifact (JAR/WAR) automatically after building.
✅ Executes the entire Maven build lifecycle (compile → test → package → install → verify → deploy).
✅ Typically used in CI/CD pipelines.
✅ Uploads the generated artifact (target/*.jar) to a remote repository (like Nexus, Artifactory, or AWS CodeArtifact).
✅ Requires the <distributionManagement> section in pom.xml to specify the remote repository URL.

- Deploy an Existing JAR to a remote repository (Example: Azure DevOps artifactory) without a Maven Project
```
mvn deploy:deploy-file -Dfile=my-library.jar \
    -DgroupId=com.example \
    -DartifactId=my-library \
    -Dversion=1.0.0 \
    -Dpackaging=jar \
    -DrepositoryId=my-repo \
    -Durl=https://repo.example.com/maven-releases
```
✅ Directly uploads a specific JAR/WAR file without running the build process.
✅ Requires manual specification of groupId, artifactId, version, packaging, and repository URL.

# Run & Test the project
## Run unit tests
```
mvn test
```
✅ Runs unit tests inside src/test/java using JUnit or TestNG.

## Run Integration tests
```
mvn verify
```
✅ Runs additional checks like integration tests and verifies the project is ready for deployment


# Other Useful Commands
## Show Dependency Tree
```
mvn dependency:tree
```
✅ Shows all dependencies, including transitive dependencies and versions.
✅ Helps in troubleshooting version conflicts.

## Find unused dependencies
```
mvn dependency:analyze
```
✅ Lists dependencies that are declared but not used and used but not declared in pom.xml.


## Running Maven in Debug mode
```
mvn clean package -X
```
✅ Runs Maven in debug mode to get detailed logs for troubleshooting.

## Skip Tests
```
mvn clean install -DskipTests
```
✅ Skips running tests during the build process.

## Run in Parallel
```
mvn clean install -T 4
```
✅ Runs the build process in parallel with 4 threads to speed up the build process.




# Maven Lifecycle Phases
mvn clean	- Deletes the target/ folder <br>
mvn compile	- Compiles Java source code <br>
mvn test	- Runs unit tests <br>
mvn package	- Creates a JAR/WAR file <br>
mvn install	- Installs the package in the local Maven repository <br>
mvn verify	- Runs integration tests <br>
mvn deploy	- Uploads the built artifact to a remote repository <br>



# Maven <scope> Values: to determine dependency visibility
- Maven manages dependencies (libraries) your project needs. But not all dependencies are needed at all times (compile time, runtime, testing, etc.). <br>
<scope> tells Maven when a dependency is required and whether it should be included in the final build artifact (like a JAR file).

- Default scope is `compile` if no scope is specified. It means the dependency is available in all build phases and included in the final artifact.  

| Scope    | Description                                                                                                                                                   | Available In           | Included In Packaged Artifact | Comments                                                                    |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|-------------------------------|-----------------------------------------------------------------------------|
| compile  | Default scope. Available in all build phases.                                                                                                                 | Compile, Test, Runtime | Yes                           | 
| provided | Available at compile and test time, but not included in the final artifact.                                                                                   | Compile, Test          | No                            | Needed to compile the code, but not needed during runtime (Example: lombok) |
| system   | Legacy; similar to `provided`, but you must explicitly provide the JAR file                                                                                   | Compile, Test          | No                            |
| runtime  | Available at runtime and test time, but not at compile time.                                                                                                  | Test, Runtime          | Yes                           | Not needed to compile the code, but needed to run it (e.g., JDBC drivers)   |
| test     | Only available during testing.                                                                                                                                | Test                   | No                            | when dependency is needed only for testing (e.g., JUnit, Mockito)           |
| import   | special; used only in <dependencyManagement> to import dependency versions from another BOM pom. Does not add a library itself, just it's dependency metadata | N/A                    | N/A                           | helps you avoid hardcoding versions in your POM                             |


Example:
- `compile` (default) scope is used for libraries like `spring-boot-starter-web` or `spring-boot-starter-data-mongodb` that are needed to compile the code and also needed at runtime.
- `provided` scope is used for libraries like `lombok` that are needed to compile the code but not needed at runtime because during compilation, lombok annotations are processed and the corresponding code is generated and included in the compiled classes. Therefore, the lombok library itself is not required at runtime.
- `test` scope is used for libraries like `junit-jupiter` and `mockito-core` that are only needed during testing.
- `runtime` scope is used for libraries like `mongodb-driver-sync` that are not needed to compile the code but are required at runtime to connect to MongoDB.
- `import` scope is used to import a BOM (Bill of Materials) like `spring-boot-dependencies` to manage versions of multiple dependencies in one place.
- `system` scope is a legacy scope and is rarely used. It requires you to provide the JAR file explicitly, which is not recommended.

```<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <scope>compile</scope> <!-- Default scope -->
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope> <!-- Needed to compile, but not at runtime -->
    </dependency>

    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope> <!-- Only needed for testing -->
    </dependency>

    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver-sync</artifactId>
        <scope>runtime</scope> <!-- Needed at runtime, not compile time -->
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>3.2.0</version>
        <type>pom</type>
        <scope>import</scope> <!-- Importing BOM for dependency versions -->
    </dependency>

    <dependency>
        <groupId>com.example</groupId>
        <artifactId>my-local-lib</artifactId>
        <version>1.0.0</version>
        <scope>system</scope> <!-- Legacy; explicitly provide the JAR file -->
        <systemPath>${project.basedir}/libs/my-local-lib-1.0.0.jar</systemPath>
    </dependency>
</dependencies>
```
