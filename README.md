# CICD-Jenkins

A minimal Java + Maven project used to practice a Jenkins CI/CD pipeline. The application code is a simple `Calculator` class with a JUnit 5 test; the interesting part is the [Jenkinsfile](Jenkinsfile) that builds, tests, and archives the artifact.

## Tech Stack

- Java 21
- Maven
- JUnit 5 (Jupiter 5.10.2)
- Maven Surefire 3.2.5
- Jenkins (declarative pipeline)

## Project Structure

```
.
├── Jenkinsfile                                  # Declarative Jenkins pipeline
├── pom.xml                                      # Maven build config
└── src
    ├── main/java/com/jaya/Calculator.java       # Application code
    └── test/java/com/jaya/CalculatorTest.java   # JUnit 5 tests
```

## Prerequisites

- JDK 21 or later
- Maven 3.9+
- Jenkins with the Maven tool configured under the name `M3` (Manage Jenkins → Tools → Maven installations)

## Build and Test Locally

```bash
# Run the tests
mvn test

# Full build: compile, test, and package the jar
mvn -B verify
```

The packaged jar is written to `target/cicd-jenkins-1.0-SNAPSHOT.jar`, and test reports land in `target/surefire-reports/`.

## Jenkins Pipeline

The [Jenkinsfile](Jenkinsfile) defines two stages:

| Stage | What it does |
| --- | --- |
| Build & Test | Runs `mvn -B verify` — compiles, runs the JUnit tests, and packages the jar |
| Archive Jar | Archives `target/*.jar` as a build artifact with fingerprinting enabled |

Post-build actions:

- **always** — publishes JUnit results from `target/surefire-reports/*.xml`
- **success** — logs `Pipeline Successful`
- **failure** — logs `Pipeline Failed`

### Setting It Up in Jenkins

1. Configure a Maven installation named `M3` in Manage Jenkins → Tools.
2. Create a new **Pipeline** job.
3. Under Pipeline, choose **Pipeline script from SCM**, select Git, and point it at `https://github.com/JayaSuriya24/cicd-jenkins.git`.
4. Set the script path to `Jenkinsfile` and the branch to `main`.
5. Save and click **Build Now**.

## Usage

```java
Calculator calculator = new Calculator();
int sum = calculator.add(2, 3); // 5
```
