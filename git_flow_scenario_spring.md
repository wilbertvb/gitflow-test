# Git Flow with a Spring Boot Project: A Step-by-Step Scenario

To understand Git Flow in a real-world scenario, we'll apply it to a "Hello World" Java Spring Boot project. This document will guide you through setting up a basic Spring Boot application and then demonstrate the Git Flow branching model, using `main` as the default production branch.

### 1. Introduction to Git Flow with Spring Boot

Git Flow is a robust branching strategy that organizes development into distinct branches for features, releases, and hotfixes, alongside primary `main` and `develop` branches. For a Spring Boot project, this structure helps manage different stages of development, from new feature implementation to production releases and urgent bug fixes, ensuring a clean and stable codebase.

### 2. Spring Boot Project Setup (Hello World)

First, let's create a simple "Hello World" Spring Boot project.

#### 2.1. Create Project using Spring Initializr

Go to [Spring Initializr](https://start.spring.io/) (https://start.spring.io/) and configure your project:

*   **Project**: Maven Project
*   **Language**: Java
*   **Spring Boot**: (Choose a stable version, e.g., 3.2.0)
*   **Group**: `com.example`
*   **Artifact**: `hello-world-app`
*   **Name**: `hello-world-app`
*   **Package name**: `com.example.helloworldapp`
*   **Packaging**: Jar
*   **Java**: (Choose a compatible Java version, e.g., 17)
*   **Dependencies**: Add `Spring Web`

Click "Generate" to download the project as a ZIP file. Extract the contents to a directory named `hello-world-app`.

#### 2.2. Project Structure Overview

The generated project will have a standard Maven/Spring Boot structure:

```
hello-world-app/
├── .mvn/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── helloworldapp/
│   │   │               └── HelloWorldAppApplication.java
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── static/
│   │       └── templates/
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── helloworldapp/
│                       └── HelloWorldAppApplicationTests.java
├── .gitignore
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

*   `pom.xml`: Maven project configuration, including dependencies.
*   `src/main/java/.../HelloWorldAppApplication.java`: The main Spring Boot application class.
*   `src/main/resources/application.properties`: Configuration file for the application.

#### 2.3. Add a "Hello World" REST Controller

Create a new Java class `HelloController.java` inside `src/main/java/com/example/helloworldapp/` with the following content:

```java
package com.example.helloworldapp;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

### 3. Git Initialization and Git Flow Setup

Now, let's initialize a Git repository and set up Git Flow.

#### 3.1. Initialize Git Repository

Navigate to the `hello-world-app` directory in your terminal and initialize Git:

```bash
git init
```

#### 3.2. Initial Commit

Add all project files and make the initial commit.

```bash
git add .
git commit -m "Initial commit of Spring Boot Hello World project"
```

#### 3.3. Set up Git Flow

We will use `main` as our production branch and `develop` as our integration branch.

*   **Create `develop` branch**:
    ```bash
    git branch develop
    ```
*   **Push to a remote repository (optional but recommended)**:
    If you have a remote repository (e.g., GitHub, GitLab), create it and then push your local branches.
    ```bash
    git remote add origin <your-remote-repository-url>
    git push -u origin main develop
    ```
*   **Using `git-flow` extension (alternative)**:
    If you have the `git-flow` extension installed, you can initialize it. When prompted, ensure `main` is set as the production branch and `develop` as the development branch.
    ```bash
    git flow init -d # -d uses default naming conventions, you can omit for interactive setup
    ```
    If you use `-d`, it will default to `master` and `develop`. You might need to manually rename `master` to `main` if it's not already.
    ```bash
    # If git flow init -d created 'master' instead of 'main'
    git branch -m master main
    git flow init # Re-initialize and set main as production branch
    ```

### 4. Git Flow in Action with Spring Boot

Let's walk through the typical Git Flow operations with our Spring Boot project.

#### 4.1. Feature Development: Add a `/greeting` Endpoint

We'll add a new endpoint `/greeting` that takes a name as a parameter.

*   **Start a new feature branch**:
    ```bash
    # Using standard Git commands
    git checkout develop
    git checkout -b feature/add-greeting-endpoint
    
    # Using git-flow extension
    git flow feature start add-greeting-endpoint
    ```
*   **Implement the feature**:
    Modify `HelloController.java` to include the new endpoint:
    ```java
    package com.example.helloworldapp;
    
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    public class HelloController {
    
        @GetMapping("/hello")
        public String hello() {
            return "Hello, Spring Boot!";
        }
    
        @GetMapping("/greeting")
        public String greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
            return String.format("Greetings, %s!", name);
        }
    }
    ```
*   **Commit changes**:
    ```bash
    git add src/main/java/com/example/helloworldapp/HelloController.java
    git commit -m "feat: Add /greeting endpoint"
    ```
*   **Finish the feature**: Merge the feature branch back into `develop`.
    ```bash
    # Using standard Git commands
    git checkout develop
    git merge feature/add-greeting-endpoint
    git branch -d feature/add-greeting-endpoint
    git push origin develop
    
    # Using git-flow extension
    git flow feature finish add-greeting-endpoint
    ```

#### 4.2. Release Management: Prepare for Version 1.0.0

Once `develop` has enough features, we prepare for a release.

*   **Start a release branch**:
    ```bash
    # Using standard Git commands
    git checkout develop
    git checkout -b release/1.0.0
    
    # Using git-flow extension
    git flow release start 1.0.0
    ```
*   **Simulate release preparation**:
    You might update the version in `pom.xml` or `application.properties`. For this example, let's just add a comment to `application.properties`.
    Edit `src/main/resources/application.properties`:
    ```properties
    # Application version 1.0.0
    ```
    Commit this change to the release branch:
    ```bash
    git add src/main/resources/application.properties
    git commit -m "chore: Prepare for release 1.0.0"
    ```
*   **Finish the release**: Merge the release branch into `main` and `develop`, and tag `main`.
    ```bash
    # Using standard Git commands
    git checkout main
    git merge release/1.0.0
    git tag -a v1.0.0 -m "Release version 1.0.0"
    git checkout develop
    git merge release/1.0.0
    git branch -d release/1.0.0
    git push origin main develop --tags
    
    # Using git-flow extension
    git flow release finish 1.0.0
    ```

#### 4.3. Hotfix Management: Fix a Typo in `/hello`

Imagine a typo is found in the `/hello` endpoint in production (which is `main`).

*   **Start a hotfix branch**:
    ```bash
    # Using standard Git commands
    git checkout main
    git checkout -b hotfix/fix-hello-typo
    
    # Using git-flow extension
    git flow hotfix start fix-hello-typo
    ```
*   **Fix the bug**:
    Modify `HelloController.java` to correct the typo (e.g., change "Hello, Spring Boot!" to "Hello from Spring Boot!").
    ```java
    package com.example.helloworldapp;
    
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    public class HelloController {
    
        @GetMapping("/hello")
        public String hello() {
            return "Hello from Spring Boot!"; // Corrected typo
        }
    
        @GetMapping("/greeting")
        public String greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
            return String.format("Greetings, %s!", name);
        }
    }
    ```
*   **Commit changes**:
    ```bash
    git add src/main/java/com/example/helloworldapp/HelloController.java
    git commit -m "fix: Correct typo in /hello endpoint"
    ```
*   **Finish the hotfix**: Merge the hotfix into `main` and `develop`, and tag `main`.
    ```bash
    # Using standard Git commands
    git checkout main
    git merge hotfix/fix-hello-typo
    git tag -a v1.0.1 -m "Hotfix for /hello typo"
    git checkout develop
    git merge hotfix/fix-hello-typo
    git branch -d hotfix/fix-hello-typo
    git push origin main develop --tags
    
    # Using git-flow extension
    git flow hotfix finish fix-hello-typo
    ```

### 5. Running the Spring Boot Application

To verify your changes at any stage, you can run the Spring Boot application.

Navigate to the `hello-world-app` directory and execute:

```bash
./mvnw spring-boot:run
```

Once the application starts (usually on `http://localhost:8080`), you can access the endpoints in your browser or using `curl`:

*   `http://localhost:8080/hello`
*   `http://localhost:8080/greeting?name=Gemini`

This step-by-step guide demonstrates how to integrate the Git Flow branching model with a "Hello World" Spring Boot project, providing a practical understanding of its application in a development workflow.