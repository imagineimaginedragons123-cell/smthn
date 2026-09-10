# SOFTWARE ENGINEERING LAB INTERNAL I — MASTER CHEAT SHEET
## Maven + Git/GitHub + Docker
### Based on the provided Set-1 to Set-6 papers and the additional Set-1 Git/Maven/Docker paper

> **Exam rule:** Copy the command, then replace placeholders such as `<repo-url>`, `<username>`, `<image>`, `<container>`, `<commit-id>`, and filenames with the values given in the question.
>
> **Important:** Some papers use `javax.servlet` (legacy Servlet API/Tomcat 9-era projects), while the AI-OLMS setup used `jakarta.servlet`/Tomcat 10.1. Do **not** mix `javax.*` and `jakarta.*` dependencies/runtime unless the question/repository specifically requires it.

---

# 0. QUICK EXAM WORKFLOW

## Maven Web Application
```bash
git clone <repo-url>
cd <project-folder>
mvn -version
java -version
javac -version
mvn validate
mvn clean package
```

Typical output:
```text
target/<artifact>.war
```

Deploy WAR manually to Tomcat:
```bash
cp target/<artifact>.war /path/to/tomcat/webapps/
```

Windows Git Bash example:
```bash
cp target/AI-OLMS.war /c/Tomcat/apache-tomcat-10.1.59/webapps/
```

Start Tomcat:
```bash
./startup.bat
```

Open:
```text
http://localhost:8080/<WAR-name-without-.war>/
```

If deployed as `ROOT.war`:
```text
http://localhost:8080/
```

---

# 1. MAVEN — CORE CONCEPTS

## What is Maven?
Maven is a Java build and dependency-management tool.

It uses:
```text
pom.xml
```

POM = Project Object Model.

Maven can:
- compile source code
- download/manage dependencies
- run tests
- package JAR/WAR files
- install artifacts into the local repository
- manage plugins
- standardize project structure

---

# 2. STANDARD MAVEN PROJECT STRUCTURE

## Java/JAR project
```text
project/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   └── test/
│       ├── java/
│       └── resources/
└── target/
```

## Maven Web Application
```text
project/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   ├── resources/
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       ├── index.jsp
│   │       └── ...
│   └── test/
│       └── java/
└── target/
```

---

# 3. CLONE A MAVEN PROJECT

```bash
git clone <repo-url>
```

Examples:
```bash
git clone https://github.com/deepthisagar7/AI-OLMS.git
git clone https://github.com/deepthisagar7/metro-reservation.git
git clone https://github.com/deepthisagar7/library-management.git
git clone https://github.com/Kumbhambhargavi75/HotelReservationSystem
git clone https://github.com/Kumbhambhargavi75/FoodSystem
git clone https://github.com/Kumbhambhargavi75/CEMS
```

Then:
```bash
cd <project-folder>
```

Verify files:
```bash
ls
```

Windows CMD:
```cmd
dir
```

Verify POM:
```bash
ls pom.xml
```

Windows CMD:
```cmd
dir pom.xml
```

---

# 4. IMPORT MAVEN PROJECT INTO ECLIPSE

### Eclipse
1. Open Eclipse.
2. `File` → `Import`.
3. Choose `Maven` → `Existing Maven Projects`.
4. Browse to the cloned project folder.
5. Select the project.
6. Click `Finish`.
7. Wait for Maven dependencies to download.
8. Check `Project Explorer` for:
   - `pom.xml`
   - `src/main/java`
   - `src/main/webapp` for web apps
   - `src/test/java`
   - `Maven Dependencies`

Alternative:
```text
File → Import → Existing Maven Projects
```

### IntelliJ
1. Open IntelliJ.
2. `File → Open`.
3. Select the project folder or `pom.xml`.
4. Select Maven import if prompted.
5. Allow dependencies to download.
6. Confirm the Maven project structure.

---

# 5. POM BASIC TEMPLATE — JAVA 17 JAR

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>library-management</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

`jar` is also Maven's default packaging, so this can be omitted:
```xml
<packaging>jar</packaging>
```

---

# 6. POM BASIC TEMPLATE — JAVA 17 WAR

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>hotel-reservation</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>war</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- Add the exact Servlet API required by the project -->
    </dependencies>

    <build>
        <finalName>HRS</finalName>
    </build>

</project>
```

---

# 7. JAVA 17 CONFIGURATION

If the question says project must use Java 17:

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

Modern alternative:
```xml
<properties>
    <maven.compiler.release>17</maven.compiler.release>
</properties>
```

For the exam, if the paper explicitly asks for source/target, use:
```xml
<maven.compiler.source>17</maven.compiler.source>
<maven.compiler.target>17</maven.compiler.target>
```

---

# 8. MAVEN LIFECYCLE PHASES

## clean
Deletes previous build output, especially:
```text
target/
```

Command:
```bash
mvn clean
```

## compile
Compiles main Java source:
```text
src/main/java
```

Output normally goes to:
```text
target/classes
```

Command:
```bash
mvn compile
```

## test
Compiles/runs unit tests.

Test classes are commonly compiled to:
```text
target/test-classes
```

JUnit/Surefire reports:
```text
target/surefire-reports
```

Command:
```bash
mvn test
```

## package
Packages the application into:
```text
JAR
```
or
```text
WAR
```
depending on `<packaging>`.

Command:
```bash
mvn package
```

## install
Installs the packaged artifact into the local Maven repository:
```text
~/.m2/repository
```

Windows:
```text
%USERPROFILE%\.m2\repository
```

Command:
```bash
mvn install
```

## Lifecycle order
If you execute:
```bash
mvn package
```
Maven executes the required earlier phases first.

If you execute:
```bash
mvn clean install
```
the effective flow is approximately:
```text
clean
→ validate
→ compile
→ test
→ package
→ install
```

---

# 9. MOST IMPORTANT MAVEN COMMANDS

```bash
mvn -version
```
Check Maven version and the Java runtime Maven is using.

```bash
java -version
```
Check Java runtime.

```bash
javac -version
```
Check Java compiler/JDK.

```bash
mvn validate
```
Validate POM/project model without doing a full build.

```bash
mvn compile
```
Compile main source.

```bash
mvn test
```
Run tests.

```bash
mvn package
```
Build JAR/WAR.

```bash
mvn clean package
```
Delete old output and build again.

```bash
mvn clean install
```
Clean, build, test, package, install into local Maven repository.

```bash
mvn clean package -X
```
Full Maven debug output.

```bash
mvn -X clean package
```
Same idea; `-X` enables debug logging.

```bash
mvn dependency:tree
```
Show direct and transitive dependencies.

```bash
mvn dependency:analyze
```
Analyze dependency usage, including potentially unused declared dependencies.

---

# 10. MAVEN DEBUGGING — JAVA VERSION MISMATCH

If:
```text
mvn clean package
```
gives an unsupported source/target error:

### Step 1 — Check Java
```bash
java -version
```

### Step 2 — Check compiler
```bash
javac -version
```

### Step 3 — Check Java used by Maven
```bash
mvn -version
```

Look for:
```text
Java version: ...
Java home: ...
```

### Step 4 — Check JAVA_HOME

Git Bash:
```bash
echo $JAVA_HOME
```

Windows CMD:
```cmd
echo %JAVA_HOME%
```

PowerShell:
```powershell
$env:JAVA_HOME
```

### Step 5 — Ensure JAVA_HOME points to a JDK
Example:
```text
C:\Program Files\Java\jdk-17
```

Git Bash:
```bash
export JAVA_HOME="/c/Program Files/Java/jdk-17"
export PATH="$JAVA_HOME/bin:$PATH"
```

Then:
```bash
mvn -version
```

### Step 6 — Ensure POM requests Java 17
```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

### Key distinction
```text
java -version
```
= Java runtime available to shell.

```text
mvn -version
```
= Java runtime Maven actually uses.

They should agree with the project requirement.

---

# 11. JDK vs JRE

Error:
```text
No compiler is provided in this environment.
Perhaps you are running on a JRE rather than a JDK?
```

Reason:
- JRE provides runtime components.
- JDK provides Java compiler (`javac`) and development tools.
- Maven compilation requires a JDK.

Check:
```bash
java -version
javac -version
mvn -version
```

If `javac` is unavailable, install/use a JDK and point `JAVA_HOME` to the JDK.

---

# 12. MAVEN BUILD OUTPUT

Common directories/files under `target/`:

```text
target/
├── classes/
├── test-classes/
├── surefire-reports/
├── <artifact>.jar
└── <artifact>.war
```

For a JAR:
```text
target/library-management.jar
```

For a WAR:
```text
target/AI-OLMS.war
```

---

# 13. DEFAULT ARTIFACT NAME

If:
```xml
<artifactId>HotelReservationSystem</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>war</packaging>
```

and there is NO `<finalName>`, Maven normally generates:
```text
HotelReservationSystem-1.0-SNAPSHOT.war
```

General rule:
```text
artifactId-version.packaging
```

Example:
```text
FoodSystem-0.0.1-SNAPSHOT.war
```

---

# 14. CUSTOM FINAL WAR/JAR NAME

If:
```xml
<build>
    <finalName>HRS</finalName>
</build>
```

then:
```text
target/HRS.war
```

If:
```xml
<finalName>AI-OLMS</finalName>
```

then:
```text
target/AI-OLMS.war
```

If:
```xml
<finalName>CEMS</finalName>
```

then:
```text
target/CEMS.jar
```
for a JAR project.

---

# 15. SNAPSHOT

Example:
```xml
<version>1.0-SNAPSHOT</version>
```

`SNAPSHOT` means the version is a development/unreleased version.

It can change as development continues.

A release version is intended to be stable/immutable, e.g.:
```text
1.0.0
```

Maven handles snapshot artifacts differently from releases and may check repositories for updated snapshot versions.

---

# 16. PACKAGING — JAR vs WAR

## JAR
Used for normal Java applications/libraries.

```xml
<packaging>jar</packaging>
```

JAR is Maven's default.

## WAR
Used for Java web applications deployed to a servlet container/application server such as Tomcat.

```xml
<packaging>war</packaging>
```

For a Tomcat web application, use WAR.

---

# 17. SERVLET API — IMPORTANT VERSION DIFFERENCE

## Legacy `javax.servlet`
Some provided papers explicitly use:
```text
javax.servlet.http.HttpServlet
```

A common Servlet 3.1 dependency:
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

Some repositories/papers may specify Servlet 4.0.1:
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

Use the version required by the actual question/project.

## Modern `jakarta.servlet`
AI-OLMS was configured with:
```xml
<dependency>
    <groupId>jakarta.servlet</groupId>
    <artifactId>jakarta.servlet-api</artifactId>
    <version>6.0.0</version>
    <scope>provided</scope>
</dependency>
```

This corresponds to the Jakarta namespace:
```java
import jakarta.servlet.*;
```

### DO NOT MIX
```text
javax.servlet.*  ↔ legacy servlet API
jakarta.servlet.* ↔ modern Jakarta Servlet API
```

A `javax.servlet` application should not simply be placed on a Jakarta-only Tomcat runtime.

---

# 18. WHY SERVLET API USES PROVIDED SCOPE

```xml
<scope>provided</scope>
```

means:
- needed for compilation
- expected to be supplied by the runtime/container
- should not normally be packaged inside the application's WAR

Tomcat supplies the servlet API at runtime.

If you package an incompatible servlet API yourself, classloading/version conflicts can occur.

---

# 19. JSTL DEPENDENCY

For a legacy `javax` JSP/JSTL application, a commonly used JSTL declaration is:

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

Use the exact JSTL coordinates/version required by the supplied project.

For Jakarta JSTL projects, use the Jakarta JSTL coordinates/version compatible with the project's Tomcat/Jakarta stack.

---

# 20. MYSQL DEPENDENCY

A modern MySQL Connector/J coordinate is commonly:

```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.0.33</version>
</dependency>
```

Older Maven examples/repositories may use:
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
```

### Exam rule
If the question says the groupId/artifactId is wrong:
1. Inspect the dependency coordinates.
2. Compare them with the actual Maven artifact expected by the project.
3. Correct both `<groupId>` and `<artifactId>`.
4. Re-run:
```bash
mvn clean package
```

DO NOT blindly replace a repository's required coordinates if the paper explicitly specifies a different artifact.

---

# 21. JUNIT DEPENDENCY

JUnit 4 example:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```

If Maven says a dependency is incomplete because `<version>` is missing, add a version.

---

# 22. POM XML TYPO DEBUGGING

Wrong:
```xml
<artificatId>servlet-api</artificatId>
```

Correct:
```xml
<artifactId>servlet-api</artifactId>
```

Correct dependency:
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
</dependency>
```

Typical POM XML problems:
- typo in `artifactId`
- malformed opening/closing tags
- duplicate/missing elements
- wrong nesting
- dependency outside `<dependencies>`
- plugin outside `<plugins>`
- missing `<groupId>`/`<artifactId>` where required

Validate:
```bash
mvn validate
```

For deeper debugging:
```bash
mvn -X validate
```

---

# 23. TOMCAT MAVEN PLUGIN

Older papers may use:
```xml
<artifactId>tomcat7-maven-plugin</artifactId>
<version>2.2</version>
```

The important missing coordinate is:
```xml
<groupId>org.apache.tomcat.maven</groupId>
```

Complete older plugin block:

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
</plugin>
```

For an actual plugin configuration, repository-specific goals/settings may also be included.

### Important
`tomcat7-maven-plugin` is an old Maven plugin. It is NOT the same thing as installing/running a modern Apache Tomcat 10.1 server.

---

# 24. `<url>` IN POM

Example:
```xml
<url>http://localhost:8080/HRS</url>
```

This does NOT tell Maven where Tomcat must deploy the application.

`<url>` is project metadata, generally representing the project's website/homepage URL.

Deployment is controlled by:
- Tomcat/server configuration
- WAR deployment
- plugin configuration, if a deployment plugin is used
- server/runtime configuration

---

# 25. `<pluginManagement>` VS `<plugins>`

## `<pluginManagement>`
Defines/defaults plugin configuration for a project or child projects.

It does not normally activate the plugin merely by being declared there.

## `<plugins>`
Actually declares the plugin for use in the current build.

Basic:
```xml
<build>
    <pluginManagement>
        <plugins>
            ...
        </plugins>
    </pluginManagement>

    <plugins>
        ...
    </plugins>
</build>
```

Exam wording:
- `pluginManagement` = configuration/defaults
- `plugins` = active plugin declaration

---

# 26. DEPENDENCY TREE

Command:
```bash
mvn dependency:tree
```

Shows:
```text
direct dependencies
transitive dependencies
dependency relationships
```

Useful for:
- checking whether a dependency exists
- finding version conflicts
- seeing which library brought a transitive dependency

Dependency conflict concept:
Maven uses dependency mediation. A direct dependency generally takes precedence over a transitive dependency, and Maven uses nearest-definition rules when resolving conflicts at different depths.

---

# 27. MAVEN LOCAL REPOSITORY

Usually:
```text
~/.m2/repository
```

Windows:
```text
%USERPROFILE%\.m2\repository
```

Git Bash:
```bash
ls ~/.m2/repository
```

Windows CMD:
```cmd
dir "%USERPROFILE%\.m2\repository"
```

---

# 28. INSTALL A CUSTOM JAR INTO LOCAL MAVEN REPOSITORY

If:
```text
library-utils.jar
```
is not in Maven Central:

```bash
mvn install:install-file \
-Dfile=library-utils.jar \
-DgroupId=com.example \
-DartifactId=library-utils \
-Dversion=1.0 \
-Dpackaging=jar
```

Windows CMD one-line form:
```cmd
mvn install:install-file -Dfile=library-utils.jar -DgroupId=com.example -DartifactId=library-utils -Dversion=1.0 -Dpackaging=jar
```

Then add to `pom.xml`:

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>library-utils</artifactId>
    <version>1.0</version>
</dependency>
```

Verify:
```bash
mvn dependency:tree
```

---

# 29. TEST OUTPUT LOCATIONS

Compiled test classes:
```text
target/test-classes
```

Surefire reports:
```text
target/surefire-reports
```

Run one test class:
```bash
mvn -Dtest=TournamentBracketTest test
```

General:
```bash
mvn -Dtest=TestClassName test
```

Skip test execution while still compiling tests:
```bash
mvn package -DskipTests
```

More aggressive:
```bash
mvn package -Dmaven.test.skip=true
```

Difference:
```text
-DskipTests
    skips test execution

-Dmaven.test.skip=true
    skips test compilation and execution
```

For exam questions asking "skip test execution", prefer:
```bash
mvn package -DskipTests
```

---

# 30. JAR APPLICATION THAT DOES NOT EXECUTE

If:
```text
target/library-management.jar
```
exists but fails to execute, possible causes:

### Cause 1 — No main method
A runnable Java application needs an entry point:
```java
public static void main(String[] args)
```

### Cause 2 — Manifest does not specify Main-Class
A JAR may need:
```text
Main-Class: com.example.Main
```

Possible Maven configuration:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.4.2</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.example.Main</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

### Cause 3 — Runtime dependencies missing
The JAR may depend on external libraries.

Check:
```bash
mvn dependency:tree
```

Then run:
```bash
java -jar target/library-management.jar
```

Read the exact error message.

---

# 31. EXECUTABLE JAR

Change:
```xml
<packaging>war</packaging>
```

to:
```xml
<packaging>jar</packaging>
```

Configure an executable JAR if needed:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.4.2</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.example.Main</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

Build:
```bash
mvn clean package
```

Run:
```bash
java -jar target/<artifact>.jar
```

---

# 32. MAVEN COMMAND TYPO

Wrong:
```bash
mvn clean pakage
```

Correct:
```bash
mvn clean package
```

Maven reports an unknown lifecycle phase because `pakage` is not a valid Maven phase.

---

# 33. TOMCAT WAR DEPLOYMENT — 3 COMMON CAUSES

If:
```text
target/app.war
```
builds but Tomcat fails:

### 1. Wrong Servlet API/runtime
Example:
```text
javax.servlet application
```
cannot automatically be treated as:
```text
jakarta.servlet application
```

Use a compatible Tomcat version and dependency namespace.

### 2. Wrong Java version
Check:
```bash
java -version
mvn -version
```

Ensure Tomcat/Maven/project use compatible Java.

### 3. WAR/deployment/configuration problem
Check:
- WAR exists
- WAR copied into `webapps`
- Tomcat startup logs
- context path
- application server status

Tomcat logs are commonly under:
```text
logs/
```

Check deployed files:
```bash
ls /usr/local/tomcat/webapps/
```

---

# 34. TOMCAT MANUAL DEPLOYMENT

Build:
```bash
mvn clean package
```

Copy:
```bash
cp target/AI-OLMS.war /c/Tomcat/apache-tomcat-10.1.59/webapps/
```

Start:
```bash
cd /c/Tomcat/apache-tomcat-10.1.59/bin
./startup.bat
```

Access:
```text
http://localhost:8080/AI-OLMS/
```

If:
```text
ROOT.war
```
is deployed:
```text
http://localhost:8080/
```

---

# 35. GIT — BASIC SETUP

If a project is NOT already a Git repository:

```bash
git init
```

Check:
```bash
git status
```

Stage:
```bash
git add .
```

Commit:
```bash
git commit -m "Initial commit"
```

Add remote:
```bash
git remote add origin <github-url>
```

Verify:
```bash
git remote -v
```

Push:
```bash
git push -u origin main
```

### IMPORTANT
If you used:
```bash
git clone <repo-url>
```
you normally DO NOT run `git init` again. `git clone` already creates the Git repository and normally creates the `origin` remote.

---

# 36. ONE-TIME GIT IDENTITY

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:
```bash
git config --global --list
```

`--global` means the setting applies to repositories for that user on the computer.

This is NOT the same as GitHub authentication.

---

# 37. GIT STATUS

```bash
git status
```

Shows:
- current branch
- staged changes
- unstaged changes
- untracked files
- merge/rebase state

Example categories:
```text
Changes to be committed       → staged
Changes not staged             → modified but unstaged
Untracked files                → not tracked by Git
```

---

# 38. CHECK BRANCHES

Local branches:
```bash
git branch
```

Local + remote:
```bash
git branch -a
```

Current branch only:
```bash
git branch --show-current
```

Remote branches:
```bash
git branch -r
```

Current branch is marked:
```text
*
```

---

# 39. CREATE AND SWITCH TO A BRANCH

Modern:
```bash
git switch -c feature/payment
```

Examples:
```bash
git switch -c feature/course-recommendation
git switch -c feature/book-management
git switch -c feature/metro-booking
git switch -c feature/player-registration
```

Older equivalent:
```bash
git checkout -b feature/payment
```

---

# 40. SWITCH BRANCHES

```bash
git switch main
```

Older:
```bash
git checkout main
```

Switch back:
```bash
git switch feature/payment
```

---

# 41. BRANCH FROM LATEST MAIN

```bash
git switch main
git pull
git switch -c feature/payment
```

If you want to explicitly fetch first:
```bash
git fetch origin
git switch main
git pull
git switch -c feature/payment
```

This creates the feature branch from the updated main.

---

# 42. ADD SPECIFIC FILE

```bash
git add Reservation.java
```

Path example:
```bash
git add src/main/java/com/sports/servlet/RegistrationServlet.java
```

Add all:
```bash
git add .
```

Commit:
```bash
git commit -m "Add player registration servlet"
```

---

# 43. VIEW EXACT CHANGES

Unstaged changes:
```bash
git diff
```

Specific file:
```bash
git diff Reservation.java
```

Specific path:
```bash
git diff src/main/webapp/index.jsp
```

Staged changes:
```bash
git diff --cached
```

or:
```bash
git diff --staged
```

---

# 44. UNSTAGE A FILE WITHOUT LOSING CHANGES

Modern:
```bash
git restore --staged Payment.java
```

Old/common:
```bash
git reset HEAD Payment.java
```

For everything:
```bash
git restore --staged .
```

The file's modifications remain in the working directory.

---

# 45. DISCARD UNCOMMITTED CHANGES COMPLETELY

If you modified:
```text
LoginServlet.java
```
and want to throw away the uncommitted modifications:

```bash
git restore LoginServlet.java
```

WARNING:
This discards the uncommitted changes to that file.

For all tracked files:
```bash
git restore .
```

---

# 46. RECOVER A DELETED FILE BEFORE COMMIT

If:
```text
Book.java
```
was deleted but deletion is not committed:

```bash
git restore Book.java
```

For an older Git version:
```bash
git checkout -- Book.java
```

---

# 47. AMEND LAST COMMIT MESSAGE

If the latest commit has NOT been pushed:

```bash
git commit --amend -m "Correct commit message"
```

Example:
```bash
git commit --amend -m "Added Assignment Module"
```

This replaces the latest commit message.

---

# 48. ADD A FILE TO THE LAST COMMIT WITHOUT CHANGING MESSAGE

Stage:
```bash
git add src/main/webapp/WEB-INF/web.xml
```

Then:
```bash
git commit --amend --no-edit
```

`--no-edit` keeps the existing commit message.

---

# 49. UNDO LAST COMMIT WHILE KEEPING CHANGES STAGED

```bash
git reset --soft HEAD~1
```

Effect:
- removes last commit
- keeps file changes
- keeps them staged in the index

Useful for "undo the last commit but keep all changes staged."

---

# 50. BAD COMMIT — NOT PUSHED

If the latest commit is local and you want to rewrite local history:

```bash
git reset --soft HEAD~1
```

Then correct changes and recommit:
```bash
git add .
git commit -m "Correct implementation"
```

If you want to completely discard the latest commit and its changes:
```bash
git reset --hard HEAD~1
```

WARNING:
`--hard` discards working-tree changes.

If only the implementation is wrong but you want to edit the commit:
```bash
git commit --amend
```

---

# 51. BAD COMMIT — ALREADY PUSHED

Preferred safe method:
```bash
git revert <commit-id>
```

Then:
```bash
git push
```

`git revert` creates a NEW commit that reverses the old commit.

Use this when the commit is already shared/pushed because it preserves history.

---

# 52. GIT MERGE

Typical feature integration:

```bash
git switch main
git pull
git merge feature/payment
git push origin main
```

---

# 53. MERGE CONFLICT — COMPLETE PROCESS

Start:
```bash
git switch main
git merge feature/payment
```

If conflict:
```bash
git status
```

Git identifies conflicted files.

Open the conflicted file.

You may see:
```text
<<<<<<< HEAD
main branch version
=======
feature branch version
>>>>>>> feature/payment
```

Manually choose/combine the correct code.

Remove ALL conflict markers:
```text
<<<<<<<
=======
>>>>>>>
```

Then stage:
```bash
git add Payment.java
```

Verify:
```bash
git status
```

Complete merge:
```bash
git commit
```

Or:
```bash
git merge --continue
```
when appropriate.

Push:
```bash
git push origin main
```

---

# 54. CANCEL A MERGE

If a merge is in progress and you want to return to the state before the merge:

```bash
git merge --abort
```

---

# 55. REMOVE FILE FROM TRACKING BUT KEEP IT LOCALLY

Example:
```text
db-config.env
```

Use:
```bash
git rm --cached src/main/resources/db-config.env
```

Then add it to `.gitignore`.

Commit:
```bash
git add .gitignore
git commit -m "Stop tracking local configuration"
```

Important:
```bash
git rm --cached
```
removes the file from Git tracking but keeps the local file.

---

# 56. UNSTAGE EVERYTHING AFTER `git add .`

```bash
git restore --staged .
```

Check:
```bash
git status
```

This does NOT delete the files or their modifications.

---

# 57. `.gitignore`

Typical Java/Eclipse/Maven entries:

```gitignore
target/
.classpath
.project
.settings/
*.class
*.log
.idea/
*.iml
.vscode/
.env
*.env
```

If the project has local DB configuration:
```gitignore
db-config.env
```

Then:
```bash
git status
```

### Important
`.gitignore` prevents untracked matching files from being added in future.

If a file is ALREADY tracked, `.gitignore` alone does not untrack it.

Use:
```bash
git rm --cached <file>
```

Then commit `.gitignore`.

---

# 58. STASH — TEMPORARILY SAVE UNCOMMITTED WORK

```bash
git stash
```

This stores uncommitted changes without creating a normal commit.

List stashes:
```bash
git stash list
```

Switch branch:
```bash
git switch main
```

Do work.

Return:
```bash
git switch feature/payment
```

Restore latest stash:
```bash
git stash pop
```

Alternative:
```bash
git stash apply
```

Difference:
```text
stash pop   → apply + remove stash if successful
stash apply → apply but keep stash
```

Restore a particular stash:
```bash
git stash apply stash@{0}
```

---

# 59. PATCH — CREATE FROM UNCOMMITTED CHANGES

For specific files:
```bash
git diff -- Reservation.java > changes.patch
```

Example:
```bash
git diff -- Event.java Registration.java Student.java > changes.patch
```

For all unstaged changes:
```bash
git diff > changes.patch
```

For STAGED changes:
```bash
git diff --cached > fooditem.patch
```

Important:
```text
git diff             → unstaged changes
git diff --cached    → staged changes
```

---

# 60. APPLY A PATCH

Check patch first:
```bash
git apply --check event.patch
```

Apply:
```bash
git apply event.patch
```

Inspect:
```bash
git apply --stat event.patch
git apply --summary event.patch
```

If parts fail:
```bash
git apply --reject --whitespace=fix event.patch
```

This may create:
```text
*.rej
```
files containing rejected hunks that must be resolved manually.

If applicable:
```bash
git apply --3way event.patch
```

---

# 61. CREATE A COMMIT PATCH

For the latest commit:
```bash
git format-patch -1 HEAD
```

For a specific commit:
```bash
git format-patch -1 <commit-id>
```

This creates an email-style patch file representing the commit.

---

# 62. GIT FETCH VS PULL

## fetch
```bash
git fetch origin
```

Downloads remote updates but does NOT merge them into your working branch.

Useful when the question says:
"retrieve remote updates without modifying local working files."

## pull
```bash
git pull
```

Normally:
```text
fetch + integrate
```

It updates the current branch.

---

# 63. UPDATE FEATURE BRANCH FROM MAIN

Option 1 — merge:
```bash
git switch feature/payment
git fetch origin
git merge origin/main
```

Option 2 — rebase:
```bash
git switch feature/payment
git fetch origin
git rebase origin/main
```

Rebase creates a linear-looking history by replaying feature commits on top of the updated main.

---

# 64. REBASE

Question wording:
"Reorganize/reorder the feature branch history so that it stems from the tip of main."

Answer:
```bash
git switch feature/payment
git fetch origin
git rebase origin/main
```

If conflicts:
```bash
git status
```

Resolve file(s).

Stage:
```bash
git add <file>
```

Continue:
```bash
git rebase --continue
```

Repeat until complete.

Cancel:
```bash
git rebase --abort
```

---

# 65. PUSH AFTER REBASE

If the rebased branch was already pushed, history has changed.

Use:
```bash
git push --force-with-lease origin feature/payment
```

Prefer:
```text
--force-with-lease
```
over:
```text
--force
```

because it provides a safer check against overwriting unexpected remote work.

If branch has never been pushed:
```bash
git push -u origin feature/payment
```

---

# 66. NON-FAST-FORWARD PUSH

If:
```text
rejected: non-fast-forward
```

Usually:
```bash
git pull --rebase origin feature/payment
```

Resolve conflicts if necessary.

Then:
```bash
git push origin feature/payment
```

Do not immediately use force push unless you understand why it is required.

---

# 67. GIT LOG

Full history:
```bash
git log
```

Compact:
```bash
git log --oneline
```

Latest commit details:
```bash
git show --stat HEAD
```

Useful:
```bash
git show HEAD
```

Branch graph:
```bash
git log --oneline --graph --decorate --all
```

Compact visual timeline:
```bash
git log --oneline --graph --decorate --all
```

---

# 68. COMPARE BRANCHES

Diff:
```bash
git diff main...feature/payment
```

Specific file:
```bash
git diff main...feature/payment -- src/main/webapp/index.jsp
```

Commits on feature not on main:
```bash
git log main..feature/payment
```

Commits on main not on feature:
```bash
git log feature/payment..main
```

---

# 69. GIT BRANCH RENAME

Rename current branch:
```bash
git branch -m feature/online-payment
```

If currently on `feature/payment`, this renames it.

Push renamed branch:
```bash
git push -u origin feature/online-payment
```

Delete old remote branch if needed:
```bash
git push origin --delete feature/payment
```

Set tracking:
```bash
git push -u origin feature/online-payment
```

Note:
`git branch -m` already removes the old local branch name because it renames it.

---

# 70. FORK + UPSTREAM + PULL REQUEST

If you do not have push permission to the original public repository:

1. Fork the repository on GitHub.
2. Clone YOUR fork:
```bash
git clone <your-fork-url>
cd <project>
```

3. Add original repository as upstream:
```bash
git remote add upstream <original-repo-url>
```

4. Verify:
```bash
git remote -v
```

5. Create feature branch:
```bash
git switch -c payment
```

6. Commit:
```bash
git add .
git commit -m "Add payment feature"
```

7. Push to your fork:
```bash
git push -u origin payment
```

8. Open a Pull Request from your fork's branch to the original repository.

---

# 71. SSH AUTHENTICATION

Generate key:
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Start agent in Git Bash:
```bash
eval "$(ssh-agent -s)"
```

Add key:
```bash
ssh-add ~/.ssh/id_ed25519
```

Display public key:
```bash
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub account SSH keys.

Test:
```bash
ssh -T git@github.com
```

SSH remote example:
```bash
git remote set-url origin git@github.com:username/repository.git
```

---

# 72. GIT TAGS

Create tag:
```bash
git tag v1.0
```

List tags:
```bash
git tag
```

Push tag:
```bash
git push origin v1.0
```

Push all tags:
```bash
git push --tags
```

Verify:
```bash
git tag
```

---

# 73. GIT REMOTE COMMANDS

Show remotes:
```bash
git remote -v
```

Get origin URL:
```bash
git remote get-url origin
```

Add:
```bash
git remote add origin <url>
```

Change:
```bash
git remote set-url origin <url>
```

---

# 74. DOCKER — BASIC CONCEPTS

Image:
```text
read-only packaged application environment
```

Container:
```text
running instance of an image
```

Image example:
```text
ai-olms:latest
```

Container example:
```text
ai-olms-container
```

Port mapping:
```text
host:container
```

Example:
```bash
-p 8080:8080
```

means:
```text
host port 8080 → container port 8080
```

---

# 75. DOCKER BUILD

Build from current directory:
```bash
docker build -t ai-olms:latest .
```

Other examples:
```bash
docker build -t metro-reservation:latest .
docker build -t library-management:latest .
docker build -t cems-app:latest .
docker build -t sportsapp-image .
```

Generic:
```bash
docker build -t <image-name>:<tag> .
```

The final:
```text
.
```
means current directory is the build context.

---

# 76. VERIFY DOCKER IMAGE

```bash
docker images
```

Or:
```bash
docker image ls
```

Specific:
```bash
docker image inspect <image>
```

---

# 77. RUN CONTAINER

Detached:
```bash
docker run -d <image>
```

Named:
```bash
docker run -d --name sports-app-container <image>
```

Port mapping:
```bash
docker run -d -p 8080:8080 <image>
```

Named + port:
```bash
docker run -d --name sports-app-container -p 8080:8080 sportsapp-image
```

---

# 78. DOCKER PORT MAPPING EXAMPLES

Host 7089 → container 8080:
```bash
docker run -d -p 7089:8080 hrs-image
```

Host 9090 → container 8080:
```bash
docker run -d -p 9090:8080 cems-app
```

Host 7456 → nginx container port 80:
```bash
docker run -d --name mynginx -p 7456:80 nginx:latest
```

Host 8080 → application container port 7114:
```bash
docker run -d -p 8080:7114 fos-image
```

---

# 79. DOCKER PS

Running containers:
```bash
docker ps
```

ALL containers:
```bash
docker ps -a
```

This includes stopped containers.

---

# 80. DOCKER LOGS

If a container crashes or application has errors:

```bash
docker logs <container>
```

Follow live logs:
```bash
docker logs -f <container>
```

This is one of the FIRST commands to use when a container exits unexpectedly.

---

# 81. DOCKER EXEC

Enter a running container:

```bash
docker exec -it <container> bash
```

If bash is unavailable:
```bash
docker exec -it <container> sh
```

Example:
```bash
docker exec -it sports-app-container bash
```

---

# 82. DOCKER INSPECT

Inspect container:
```bash
docker inspect <container>
```

Useful state information:
```bash
docker inspect --format='{{.State.Status}} {{.State.ExitCode}} {{.State.Error}}' <container>
```

Check:
- running/stopped state
- exit code
- error
- network
- mounts
- configuration

---

# 83. DOCKER START / STOP / REMOVE

Stop:
```bash
docker stop <container>
```

Start:
```bash
docker start <container>
```

Restart:
```bash
docker restart <container>
```

Remove:
```bash
docker rm <container>
```

Force stop + remove:
```bash
docker rm -f <container>
```

---

# 84. DOCKER CP

Copy WAR into a running Tomcat container:

```bash
docker cp target/AI-OLMS.war <container>:/usr/local/tomcat/webapps/
```

Verify:
```bash
docker exec <container> ls /usr/local/tomcat/webapps/
```

---

# 85. DOCKER PULL

Pull image:
```bash
docker pull ubuntu
```

Nginx:
```bash
docker pull nginx:latest
```

Tomcat:
```bash
docker pull tomcat:<compatible-tag>
```

Python:
```bash
docker pull python
```

---

# 86. RUN UBUNTU INTERACTIVELY

```bash
docker run -it ubuntu bash
```

Named:
```bash
docker run -it --name labinternal-1 ubuntu bash
```

Detached Ubuntu:
```bash
docker run -d --name labinternal-1 ubuntu
```

Then enter:
```bash
docker exec -it labinternal-1 bash
```

---

# 87. UBUNTU — INSTALL GIT AND NANO

Inside Ubuntu:

```bash
apt update
apt install -y git nano
```

Verify:
```bash
git --version
nano --version
```

---

# 88. PYTHON CONTAINER

Pull:
```bash
docker pull python
```

Run:
```bash
docker run -it python
```

Verify:
```bash
python --version
```

If using Ubuntu instead:
```bash
docker pull ubuntu
docker run -it --name ubuntu-python ubuntu bash
```

Inside:
```bash
apt update
apt install -y python3
python3 --version
python3 -c "print('Hello from Python')"
```

---

# 89. DOCKERFILE — JAR APPLICATION

For a Java 17 application where the JAR is already generated:

```dockerfile
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY target/library-management.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

Build:
```bash
mvn clean package
docker build -t library-management:latest .
```

Run:
```bash
docker run -d --name library-management-container -p 8080:8080 library-management:latest
```

---

# 90. DOCKERFILE — CEMS/JAVA 21 JAR

If the question explicitly requires Java 21 and the generated JAR is:
```text
target/CEMS.jar
```

Runtime-only Dockerfile:
```dockerfile
FROM eclipse-temurin:21-jre

WORKDIR /app

COPY target/CEMS.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

If the question says "copy the JAR and run", this simple form is usually sufficient.

---

# 91. MULTI-STAGE DOCKERFILE — MAVEN + TOMCAT WAR

Generic template:

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

FROM tomcat:10.1-jdk17

COPY --from=build /app/target/AI-OLMS.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
```

Why multi-stage?
```text
Stage 1 = Maven build
Stage 2 = Tomcat runtime
```

The final image does not need the Maven build environment.

### If the exam specifically requires the generated WAR filename
Replace:
```text
AI-OLMS.war
```
with:
```text
metro-reservation.war
```
or the filename in the question.

---

# 92. TOMCAT WAR DEPLOYMENT INSIDE DOCKER

Normal:
```dockerfile
COPY target/metro-reservation.war /usr/local/tomcat/webapps/
```

The application normally becomes available under:
```text
/metro-reservation/
```

If the question specifically says application should be available directly at:
```text
http://localhost:8080
```
you can deploy as:
```dockerfile
COPY target/metro-reservation.war /usr/local/tomcat/webapps/ROOT.war
```

Then:
```text
http://localhost:8080/
```

This avoids needing the WAR name as the context path.

---

# 93. MULTI-STAGE DOCKERFILE FOR METRO RESERVATION

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

FROM tomcat:10.1-jdk17

COPY --from=build /app/target/metro-reservation.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
```

Build:
```bash
docker build -t metro-reservation:latest .
```

Run:
```bash
docker run -d --name metro-reservation-container -p 8080:8080 metro-reservation:latest
```

---

# 94. MULTI-STAGE DOCKERFILE FOR AI-OLMS

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

FROM tomcat:10.1-jdk17

COPY --from=build /app/target/AI-OLMS.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
```

Build:
```bash
docker build -t ai-olms:latest .
```

Run:
```bash
docker run -d --name ai-olms-container -p 8080:8080 ai-olms:latest
```

---

# 95. IF EXAM WANTS THE WAR NAME PRESERVED

Use:
```dockerfile
COPY --from=build /app/target/AI-OLMS.war /usr/local/tomcat/webapps/
```

Then access:
```text
http://localhost:8080/AI-OLMS/
```

If the exam explicitly says:
```text
http://localhost:8080
```
prefer:
```dockerfile
COPY ... /usr/local/tomcat/webapps/ROOT.war
```

---

# 96. DOCKER 404 TROUBLESHOOTING

Container is running but browser says:
```text
404 Not Found
```

Check 1 — Is WAR present?
```bash
docker exec <container> ls -l /usr/local/tomcat/webapps/
```

Check 2 — Is Tomcat running?
```bash
docker logs <container>
```

Check 3 — Did WAR unpack?
Look for:
```text
<war-name>/
```

inside:
```text
/usr/local/tomcat/webapps/
```

Check 4 — Correct context path?
If:
```text
AI-OLMS.war
```
normally try:
```text
http://localhost:8080/AI-OLMS/
```

If:
```text
ROOT.war
```
try:
```text
http://localhost:8080/
```

Check 5 — Is the application actually serving that URL?
A running Tomcat container does NOT guarantee your application has a matching route.

---

# 97. DOCKER CANNOT ACCESS `localhost:8080`

Run:
```bash
docker ps
```

Check:
```text
PORTS
```

Should show something like:
```text
0.0.0.0:8080->8080/tcp
```

Check logs:
```bash
docker logs <container>
```

Check process:
```bash
docker exec <container> ps
```

Check container state:
```bash
docker inspect <container>
```

Check listening/deployment files where appropriate:
```bash
docker exec <container> ls /usr/local/tomcat/webapps/
```

Also verify:
- container is running
- correct host/container ports
- application listens on expected internal port
- firewall/host port is not already occupied
- Tomcat/application startup did not fail

---

# 98. DOCKER HUB — TAG

Generic:
```bash
docker tag <local-image>:<tag> <username>/<repository>:<tag>
```

Example:
```bash
docker tag cems-app:latest yourusername/cems-app:latest
```

Sports:
```bash
docker tag sportsapp-image yourusername/sportsapp:v1
```

Hotel:
```bash
docker tag hrs-image yourusername/hotel-reservation:latest
```

---

# 99. DOCKER LOGIN + PUSH

Login:
```bash
docker login
```

Push:
```bash
docker push yourusername/cems-app:latest
```

Example:
```bash
docker push yourusername/hotel-reservation:latest
```

Verify:
```bash
docker pull yourusername/hotel-reservation:latest
```

Or check the public repository on Docker Hub.

---

# 100. SAVE A CONTAINER AS A NEW IMAGE

If question says:
"Save the state of container ID ... into an image"

Use:
```bash
docker commit 0e993d2009a1 yourusername/sportsapp:v1
```

Verify:
```bash
docker images
```

Then login:
```bash
docker login
```

Push:
```bash
docker push yourusername/sportsapp:v1
```

---

# 101. REMOVE STORED DOCKER AUTHENTICATION

Normal logout:
```bash
docker logout
```

This logs out the current Docker registry.

For the exam wording:
"Terminate your session and remove stored authentication credentials"
the expected command is:
```bash
docker logout
```

---

# 102. COMPLETE DOCKER WORKFLOW — JAR

```bash
git clone <repo-url>
cd <project>
mvn clean package

docker build -t library-management:latest .

docker images

docker run -d \
  --name library-management-container \
  -p 8080:8080 \
  library-management:latest

docker ps

docker logs library-management-container

docker exec -it library-management-container sh
```

---

# 103. COMPLETE DOCKER WORKFLOW — WAR/TOMCAT

```bash
git clone <repo-url>
cd <project>

mvn clean package

docker build -t ai-olms:latest .

docker images

docker run -d \
  --name ai-olms-container \
  -p 8080:8080 \
  ai-olms:latest

docker ps

docker logs ai-olms-container

docker exec -it ai-olms-container sh

docker exec ai-olms-container ls /usr/local/tomcat/webapps/
```

---

# 104. COMPLETE GIT WORKFLOW

If project is not initialized:

```bash
git init
git status
git add .
git commit -m "Initial commit"
git remote add origin <github-url>
git push -u origin main
```

Feature:

```bash
git switch main
git pull
git switch -c feature/payment
```

Work:
```bash
git status
git diff
```

Stage:
```bash
git add Payment.java
git status
```

Commit:
```bash
git commit -m "Add payment feature"
```

Push:
```bash
git push -u origin feature/payment
```

Merge:
```bash
git switch main
git pull
git merge feature/payment
git push origin main
```

---

# 105. COMPLETE REBASE WORKFLOW

```bash
git switch main
git pull

git switch feature/payment
git fetch origin
git rebase origin/main
```

If conflict:
```bash
git status
```

Edit files.

Then:
```bash
git add <resolved-file>
git rebase --continue
```

Repeat.

Cancel:
```bash
git rebase --abort
```

After successful rebase:
```bash
git push --force-with-lease origin feature/payment
```

---

# 106. COMMON EXAM COMMAND MAPPING

| Question wording | Command |
|---|---|
| Clone repository | `git clone <url>` |
| Initialize Git | `git init` |
| Check status | `git status` |
| Add all | `git add .` |
| Commit | `git commit -m "message"` |
| Add remote | `git remote add origin <url>` |
| Verify remote | `git remote -v` |
| Push main | `git push -u origin main` |
| Correct latest unpushed message | `git commit --amend -m "..."` |
| Create + switch branch | `git switch -c branch` |
| Switch branch | `git switch branch` |
| Restore deleted uncommitted file | `git restore file` |
| Unstage file | `git restore --staged file` |
| Unstage everything | `git restore --staged .` |
| Discard uncommitted file changes | `git restore file` |
| Undo pushed commit safely | `git revert <commit>` |
| Undo last commit, keep staged | `git reset --soft HEAD~1` |
| Cancel merge | `git merge --abort` |
| Cancel rebase | `git rebase --abort` |
| Temporarily save work | `git stash` |
| List stashes | `git stash list` |
| Restore stash | `git stash pop` |
| Fetch remote only | `git fetch origin` |
| Update current branch | `git pull` |
| Update feature with main by rebase | `git rebase origin/main` |
| View differences | `git diff` |
| View staged differences | `git diff --cached` |
| Compare branches | `git diff main...feature` |
| View history | `git log` |
| Compact history | `git log --oneline` |
| Visual history | `git log --oneline --graph --decorate --all` |
| List branches | `git branch` |
| Local + remote branches | `git branch -a` |
| Current branch | `git branch --show-current` |
| Stop tracking, keep local | `git rm --cached file` |
| Create unstaged patch | `git diff > changes.patch` |
| Create staged patch | `git diff --cached > changes.patch` |
| Check patch | `git apply --check file.patch` |
| Apply patch | `git apply file.patch` |
| Commit patch | `git format-patch -1 HEAD` |
| Rename branch | `git branch -m new-name` |
| Push branch + tracking | `git push -u origin branch` |
| Create tag | `git tag v1.0` |
| Push tag | `git push origin v1.0` |
| Maven version/Java used | `mvn -version` |
| Java version | `java -version` |
| Compiler version | `javac -version` |
| Validate POM | `mvn validate` |
| Compile | `mvn compile` |
| Test | `mvn test` |
| Package | `mvn package` |
| Clean package | `mvn clean package` |
| Full debug build | `mvn clean package -X` |
| Dependency tree | `mvn dependency:tree` |
| Dependency analysis | `mvn dependency:analyze` |
| Skip test execution | `mvn package -DskipTests` |
| Install artifact locally | `mvn install` |
| Install custom JAR | `mvn install:install-file ...` |
| Docker build | `docker build -t image:tag .` |
| List images | `docker images` |
| Run detached | `docker run -d image` |
| Port mapping | `docker run -d -p host:container image` |
| Running containers | `docker ps` |
| All containers | `docker ps -a` |
| Logs | `docker logs container` |
| Enter container | `docker exec -it container bash` |
| Inspect container | `docker inspect container` |
| Stop | `docker stop container` |
| Restart | `docker restart container` |
| Remove | `docker rm container` |
| Stop + remove | `docker rm -f container` |
| Pull image | `docker pull image` |
| Copy file into container | `docker cp source container:path` |
| Tag image | `docker tag local username/repo:tag` |
| Docker Hub login | `docker login` |
| Docker Hub push | `docker push username/repo:tag` |
| Logout | `docker logout` |
| Container → image | `docker commit container image:tag` |

---

# 107. 🆕 VARIATIONS THAT APPEAR IN DIFFERENT PAPERS

## Maven variations
- Java 6 → Java 17 migration
- Java 8 → Java 17 migration
- Java 17 → Java 21 migration
- JAR instead of WAR
- WAR instead of JAR
- JUnit version missing
- MySQL coordinates incorrect
- dependency `<artifactId>` typo
- entire `<dependencies>` section removed
- JDK vs JRE problem
- custom/vendor JAR installation
- `<finalName>` customization
- default artifact naming
- `<url>` meaning
- `tomcat7-maven-plugin` missing groupId
- `pluginManagement` vs `plugins`
- `mvn validate`
- `mvn -X clean package`
- `mvn dependency:tree`
- `mvn dependency:analyze`
- test output paths
- run a single test
- skip tests
- executable JAR

## Git variations
- normal init/commit/push
- amend commit message
- branch creation
- restore deleted file
- merge conflict
- merge abort
- rebase
- rebase conflict
- rebase abort
- revert pushed commit
- reset unpushed commit
- soft reset keeping staged changes
- unstage
- `.gitignore`
- `git rm --cached`
- stash/apply/pop
- patches
- apply patches
- `format-patch`
- branch comparison
- commit graph
- branch rename
- tags
- fork + upstream + pull request
- SSH authentication
- non-fast-forward
- force-with-lease after rebase
- cherry-pick may be useful when moving an existing commit between branches

## Docker variations
- JAR + Java runtime
- WAR + Tomcat
- multi-stage Maven/Tomcat
- Java 17
- Java 21
- Ubuntu
- Python
- Nginx
- port mapping
- logs
- exec
- inspect
- stop/restart
- copy WAR
- tag/push
- commit container to image
- Docker logout

---

# 108. GIT SCENARIO — CHANGES COMMITTED ON WRONG BRANCH

If a commit was accidentally made on `main` but should be on a feature branch, one approach is:

1. Identify commit:
```bash
git log --oneline
```

2. Create feature branch at current commit:
```bash
git branch event
```

3. Move main back if that accidental commit should not remain on main:
```bash
git switch main
git reset --hard HEAD~1
```

4. Switch to feature:
```bash
git switch event
```

If multiple commits were made, use the appropriate number:
```bash
git reset --hard HEAD~N
```

WARNING:
`reset --hard` is appropriate only when those commits are local/unshared and you understand the history change.

---

# 109. CHERRY-PICK

If you need one existing commit on another branch:

```bash
git cherry-pick <commit-id>
```

Example scenario:
```text
commit exists on main
but should also be applied to feature branch
```

Then:
```bash
git switch feature/payment
git cherry-pick <commit-id>
```

If conflicts:
```bash
git status
```

Resolve, then:
```bash
git add <file>
git cherry-pick --continue
```

Abort:
```bash
git cherry-pick --abort
```

---

# 110. FINAL 30-SECOND MAVEN MEMORY

```text
pom.xml
   ↓
validate
   ↓
compile
   ↓
test
   ↓
package
   ↓
install
```

Commands:
```bash
mvn validate
mvn compile
mvn test
mvn package
mvn install
```

Most common:
```bash
mvn clean package
```

Debug:
```bash
mvn clean package -X
```

Dependencies:
```bash
mvn dependency:tree
```

Java:
```bash
java -version
javac -version
mvn -version
```

---

# 111. FINAL 30-SECOND GIT MEMORY

```text
clone/init
   ↓
status
   ↓
add
   ↓
commit
   ↓
branch
   ↓
work
   ↓
diff/status
   ↓
commit
   ↓
push
```

Conflict:
```text
status
→ edit conflict
→ git add
→ git commit / rebase --continue
```

Undo:
```text
UNPUSHED → reset/amend
PUSHED   → revert
```

Temporary:
```bash
git stash
git stash list
git stash pop
```

Rebase:
```bash
git fetch origin
git rebase origin/main
```

---

# 112. FINAL 30-SECOND DOCKER MEMORY

Build:
```bash
docker build -t app:latest .
```

Check:
```bash
docker images
```

Run:
```bash
docker run -d -p 8080:8080 app:latest
```

Check:
```bash
docker ps
```

Logs:
```bash
docker logs <container>
```

Enter:
```bash
docker exec -it <container> bash
```

Inspect:
```bash
docker inspect <container>
```

Push:
```bash
docker tag app:latest username/app:latest
docker login
docker push username/app:latest
```

---

# 113. EXAM-READY ANSWER PATTERNS

## "Identify 3 causes and troubleshoot"
Write:
1. Cause.
2. Check command.
3. Fix.

Example:
```text
1. Java mismatch
   java -version
   mvn -version
   Configure JAVA_HOME/POM to Java 17.

2. Wrong Servlet API/runtime
   Check pom.xml and Tomcat version.
   Use compatible javax/jakarta stack.

3. Deployment problem
   Check target/*.war, Tomcat webapps, and logs.
```

## "Write complete commands"
Do not omit:
```text
cd project
```
when required.

For Docker:
```text
build → images → run → ps → logs
```

For Git:
```text
status → add → commit → push
```

---

# 114. HIGH-RISK EXAM TRAPS

### Trap 1
```bash
git clone ...
git init
```
Do NOT initialize again after cloning unless the question specifically asks for a separate repository initialization.

### Trap 2
```bash
git restore file
```
DISCARDS uncommitted modifications.

```bash
git restore --staged file
```
ONLY unstages it; modifications remain.

### Trap 3
```bash
git reset --hard
```
Can discard working-tree changes. Use carefully.

### Trap 4
Pushed bad commit:
```bash
git revert <commit>
```
Usually safer than rewriting shared history.

### Trap 5
After rebase of an already-pushed feature:
```bash
git push --force-with-lease
```

### Trap 6
`.gitignore` does not automatically untrack an already tracked file.

Use:
```bash
git rm --cached <file>
```

### Trap 7
`git fetch` does NOT normally change your working files.

`git pull` fetches and integrates changes.

### Trap 8
`mvn package` creates the artifact.

For WAR:
```text
target/*.war
```

For JAR:
```text
target/*.jar
```

### Trap 9
Docker port syntax is:
```text
-p HOST:CONTAINER
```

### Trap 10
Nginx normally listens on:
```text
80
```
So:
```bash
-p 7456:80
```
not:
```bash
-p 7456:8080
```

### Trap 11
A WAR filename affects Tomcat context path.

```text
AI-OLMS.war
→ /AI-OLMS/
```

```text
ROOT.war
→ /
```

### Trap 12
`javax.servlet` and `jakarta.servlet` are different namespaces and must match the server/runtime.

---

# 115. MINIMAL `.gitignore` FOR JAVA/MAVEN/ECLIPSE

```gitignore
target/
*.class

.classpath
.project
.settings/

.idea/
*.iml

.vscode/

*.log
*.env
.env
```

---

# 116. MINIMAL JAR DOCKERFILE

```dockerfile
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY target/library-management.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

# 117. MINIMAL WAR/TOMCAT DOCKERFILE

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

FROM tomcat:10.1-jdk17

COPY --from=build /app/target/AI-OLMS.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
```

---

# 118. ONE-PAGE COMMAND BLOCK

```bash
# GIT
git clone <url>
cd <project>
git status
git branch -a
git remote -v
git switch -c feature/payment
git diff
git add .
git status
git commit -m "message"
git push -u origin feature/payment

# GIT UNDO / RECOVERY
git restore <file>
git restore --staged <file>
git restore --staged .
git commit --amend -m "message"
git reset --soft HEAD~1
git revert <commit-id>
git rm --cached <file>
git merge --abort
git rebase --abort

# GIT REMOTE / UPDATE
git fetch origin
git pull
git pull --rebase
git rebase origin/main
git rebase --continue
git push --force-with-lease

# GIT STASH / PATCH
git stash
git stash list
git stash pop
git diff > changes.patch
git diff --cached > changes.patch
git apply --check changes.patch
git apply changes.patch
git format-patch -1 HEAD

# MAVEN
java -version
javac -version
mvn -version
mvn validate
mvn compile
mvn test
mvn package
mvn clean package
mvn clean install
mvn clean package -X
mvn dependency:tree
mvn dependency:analyze
mvn -Dtest=TestClassName test
mvn package -DskipTests

# CUSTOM JAR
mvn install:install-file -Dfile=library-utils.jar -DgroupId=com.example -DartifactId=library-utils -Dversion=1.0 -Dpackaging=jar

# DOCKER
docker build -t app:latest .
docker images
docker run -d --name app-container -p 8080:8080 app:latest
docker ps
docker ps -a
docker logs app-container
docker exec -it app-container bash
docker inspect app-container
docker stop app-container
docker start app-container
docker restart app-container
docker rm app-container
docker rm -f app-container
docker pull ubuntu
docker cp target/app.war app-container:/usr/local/tomcat/webapps/
docker tag app:latest username/app:latest
docker login
docker push username/app:latest
docker logout
docker commit <container> username/app:v1
```

---

# 119. LAST-MINUTE CHECKLIST

Before submitting an answer, check:

## Maven
- [ ] `pom.xml` is valid XML.
- [ ] Correct `<groupId>`.
- [ ] Correct `<artifactId>`.
- [ ] Correct `<version>`.
- [ ] Correct `<packaging>`.
- [ ] Java version matches requirement.
- [ ] Dependencies have correct coordinates/version.
- [ ] Servlet scope is correct.
- [ ] `<finalName>` is correct if asked.
- [ ] Run `mvn clean package`.

## Git
- [ ] Correct branch.
- [ ] `git status`.
- [ ] Correct files staged.
- [ ] Correct commit message.
- [ ] Correct remote.
- [ ] Pull/fetch before integrating remote work.
- [ ] Resolve conflict markers completely.
- [ ] Stage resolved files.
- [ ] Push after merge.
- [ ] Use `revert` for a pushed bad commit.
- [ ] Use `reset/amend` for local unpushed history changes.

## Docker
- [ ] Correct base image.
- [ ] Correct JAR/WAR path.
- [ ] Correct working directory.
- [ ] Correct Tomcat webapps path for WAR.
- [ ] Correct `EXPOSE`.
- [ ] Correct `ENTRYPOINT`/startup command.
- [ ] `docker build`.
- [ ] `docker images`.
- [ ] `docker run -d`.
- [ ] Correct `-p HOST:CONTAINER`.
- [ ] `docker ps`.
- [ ] `docker logs` if broken.
- [ ] Check deployment/context path for 404.
- [ ] Tag before Docker Hub push.

---

# END OF MASTER CHEAT SHEET
