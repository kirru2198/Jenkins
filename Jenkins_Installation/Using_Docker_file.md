# Installing Jenkins with Blue Ocean Using Docker

## Step 1: Pull the Jenkins Blue Ocean Docker Image

Use the following command to pull the Jenkins Blue Ocean image from Docker Hub:

```bash
docker pull jenkinsci/blueocean
```

## Step 2: Run Jenkins Container

Create and run a Jenkins container using the following Docker command:

```bash
sudo docker run \
  -p 8080:8080 \
  -p 50001:50001 \
  -v /home/<your-username>/Desktop/jenkins_home:/home/jenkins_home \
  jenkinsci/blueocean
```

Replace `<your-username>` with your actual system username.

## Step 3: Access Jenkins

* Open a browser and visit: [http://localhost:8080](http://localhost:8080)
* Copy the admin password from the Jenkins container log output.
* Paste the password in the browser to proceed with the setup wizard.
* After the setup, visit: [http://localhost:8080/blue](http://localhost:8080/blue) to access the Blue Ocean interface.

## Additional Reference

Visit the official Jenkins installation documentation: [https://www.jenkins.io/doc/book/installing/](https://www.jenkins.io/doc/book/installing/)

---

# Dockerfile for Docker Agent in Declarative Pipeline

## Dockerfile

```Dockerfile
# Clone the application repository
FROM alpine/git as clone
WORKDIR /app
RUN git clone <repository-url>

# Use Maven to build the Java application
FROM maven:3.6.3-jdk-8 as build
WORKDIR /app
COPY --from=clone /app/sample-java-app/app .
```

Replace `<repository-url>` with the actual Git repository URL of your project.

---

# Jenkins Declarative Pipeline using Dockerfile Agent

## Jenkinsfile

```groovy
pipeline {
  agent {
    dockerfile true
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'

        // Publish JUnit test report
        junit '**/target/surefire-reports/TEST-*.xml'

        // Archive WAR file artifacts
        archiveArtifacts(
          artifacts: 'target/**/*.war',
          onlyIfSuccessful: true,
          fingerprint: true
        )
      }
    }
  }
}
```

---

# Jenkins Console Output Log (Summarized)

## Initial Setup

```
Obtained Jenkinsfile from dfe1102804f5b906ba5b4b49cb966f10e960c1ab
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/java-sample-web_docker
```

## Git Checkout and Clone

```
Cloning repository <your-git-repository-url>
> git init
> git clone <repository-url>
```

## Dockerfile Execution

```
Step 1/6 : FROM alpine/git as clone ---> f54f496311fb
Step 2/6 : WORKDIR /app ---> c8ba2ced5cc0
Step 3/6 : RUN git clone <repository-url> ---> 7cb7be0c3d5f
Step 4/6 : FROM maven:3.6.3-jdk-8 as build ---> 97495355e4f9
Step 5/6 : WORKDIR /app ---> 18c34e2d0e6d
Step 6/6 : COPY --from=clone /app/sample-java-app/app . ---> 41edde9ae237
Successfully built 41edde9ae237
Successfully tagged <image-tag>
```

## Maven Build

```
+ mvn clean package
[INFO] Scanning for projects...
[INFO] Downloading dependencies...
[INFO] Running Tests...
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Tests run: 59, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
[INFO] Total time: 08:48 min
```

## Post-Build Actions

```
Recording test results
Archiving artifacts
Recording fingerprints
Finished: SUCCESS
```

---

This document describes how to:

* Set up Jenkins with Blue Ocean using Docker.
* Use a Dockerfile as a build agent.
* Configure a Jenkins pipeline for a sample Java application.
* Observe key log outputs from Jenkins during build and execution.

All steps are intended to help you set up and automate your CI/CD pipeline using Jenkins and Docker effectively.
