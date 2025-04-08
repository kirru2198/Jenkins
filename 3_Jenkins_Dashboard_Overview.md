## Jenkins Dashboard Overview

The **Jenkins dashboard** is the central control panel that provides a summary of your Jenkins environment. It shows an overview of running jobs, builds, and gives you quick access to Jenkins features. On the **left pane**, you'll find navigation links that guide you to various Jenkins sections.

### Key Features of the Dashboard:

- **Manage Jenkins**: Allows you to configure Jenkins settings.
- **New Item**: Helps you create new jobs.
- **Plugins**: Manages Jenkins plugins.
- **Other Links**: Includes options to manage Jenkins security, system configuration, and build history.

The UI is designed to be simple and user-friendly, making it easy to interact with Jenkins. The current section you're in is indicated at the top of the page (e.g., **Dashboard > Manage Jenkins > Plugins**). You can easily navigate back to the dashboard by clicking "Dashboard."

> **Tip:** If you're not familiar with Jenkins, take time to explore the left navigation pane and familiarize yourself with the layout. This will make working with Jenkins much easier.

---

## Creating a Simple Job in Jenkins

### Objective:
To understand how Jenkins works, we’ll create a simple job. This will also introduce the concept of build automation tools.

#### Build Automation Tools Overview

Build automation tools are essential for automating tasks in the software development lifecycle, such as:
- **Compiling code**
- **Testing code**
- **Packaging the code**
- **Managing dependencies**

Without these tools, developers would manually handle tasks like compiling, testing, and packaging, which is time-consuming and error-prone. Build automation tools save time and reduce errors.

**Why are these tools important?**  
- **Efficiency**: Automates repetitive tasks.
- **Standardization**: Ensures that everyone on the team follows the same process.
- **Dependency Management**: Automatically handles required libraries and software packages.

---

## Introduction to Apache Maven

Apache **Maven** is one of the most popular build automation tools used, especially for Java projects. It simplifies the build process by managing dependencies, compiling code, running tests, and packaging the application.

### How Maven Works:
- **POM (Project Object Model)**: The `pom.xml` file is central to Maven's operation. It defines:
  - **Project information**: Name, version, etc.
  - **Dependencies**: External libraries that the project needs.
  - **Plugins**: Tools needed for tasks like compiling, testing, and packaging.
  - **Lifecycle Configuration**: Defines tasks for each project stage (e.g., compiling, testing).

Maven automatically handles dependencies, downloading any required libraries, and managing the build lifecycle.

---

## Role of Jenkins in Automation

While Maven automates tasks like compiling and testing, **Jenkins** automates the entire process by:
- **Triggering builds automatically** when code is pushed to GitHub or any other repository.
- **Integrating with various tools** to streamline the CI/CD pipeline.
- **Managing jobs** to compile, test, and deploy the application.

Without Jenkins, tasks like compiling, testing, and packaging would have to be triggered manually, leading to inefficiency.

---

## Setting Up Jenkins with Maven

To work with Jenkins and Maven, we need to ensure the following tools are installed on the Jenkins machine:
1. **Java** (since our code is Java-based)
2. **Maven** (for build automation)
3. **Git** (for accessing code stored on GitHub)

You can install these tools in Jenkins in two ways:

### Method 1: Automatic Installation
- Jenkins can automatically install Java, Maven, and Git for you.
- To set this up, go to **Manage Jenkins > Configure System**.
- Under **Tools**, select **Install Automatically** for tools like Maven, JDK, and Git.

### Method 2: Manual Installation
- If you already have Java, Maven, and Git installed, provide Jenkins with the path to these tools.
- Set the **JAVA_HOME** environment variable to the path where Java is installed (e.g., `/usr/lib/jvm/java-17-openjdk`).
- Update the **path** variable to help Jenkins locate these tools when needed.

> **Note:** Setting up tools properly ensures Jenkins can successfully compile and run your code.

---

## Creating a Jenkins Job

Let’s create a Jenkins job to compile code using Maven:

### Steps for Creating the Job:
1. **Create a New Item**: In the Jenkins dashboard, click **New Item** to create a new job.
2. **Source Code Management**:
   - Select **Git** and provide the URL to the GitHub repository where the code is stored.
   - If the repository is private, provide the necessary credentials.
3. **Branch Configuration**:
   - Define the Git branch to work with (e.g., **main**).
4. **Build Trigger**:
   - Set up triggers to automatically start the build process, such as after a code push to GitHub.
5. **Build Steps**:
   - Choose **Invoke top-level Maven targets**.
   - Select the Maven version and set the Maven goal (e.g., `clean install`).
6. **Post-Build Actions**:
   - Define actions like deploying the application after a successful build.

---

## Integration with GitHub

Jenkins will automatically compile the code when a push happens to the GitHub repository. The following needs to be set up:
1. **GitHub Repository URL**: Provide the Git URL where the project is hosted.
2. **Branch Name**: Specify the branch (e.g., **main**).
3. **Credentials**: If required, set up credentials to access the private Git repository.

---

## Summary

In this session, we’ve covered:
- **Setting up Jenkins**: We learned how to install and configure Jenkins on a Linux machine.
- **Creating a Simple Job**: We created a simple Jenkins job to understand its functionality.
- **Using Build Automation Tools**: We explored the importance of build tools like **Maven** for automating the build process.
- **Integrating Jenkins with GitHub**: We learned how to integrate Jenkins with GitHub for automated builds.

### What’s Next:
- **Build Triggers**: We’ll go deeper into setting up build triggers in Jenkins.
- **GitHub Integration**: More details on connecting Jenkins to GitHub repositories.
- **Complex Projects**: We will continue to explore more complex Jenkins functionality and real-world CI/CD pipelines.

---

### Final Notes:
- Jenkins is an essential tool for automating software deployment.
- Build automation tools like Maven and Gradle help streamline development processes.
- With Jenkins, you can fully automate the build, test, and deployment cycle, enabling continuous integration and continuous delivery (CI/CD).
