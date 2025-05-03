## Build Automation Tools: Maven

- Build tools like **Maven**, **Gradle**, and **Make** help automate tasks like compiling source code, testing it, and converting it into packages.
- **Maven** is the tool we’ll focus on since we are using **Java** for our examples.
  - **Maven** uses a **POM (Project Object Model)** file, which contains project details like directory structure, dependencies, and configuration.
  - **Maven** reads this file and ensures all necessary dependencies are downloaded automatically.

---

## Using Jenkins and Maven Together

We will create small jobs to understand how **Jenkins** and **Maven** work together to automate the build process. These jobs will help us build the project step by step. Once we understand the basic concepts, we’ll gradually make the project more complex.

### Steps to Create a Jenkins Job for Compilation

1. **Install Dependencies on Jenkins Machine:**
   - The following dependencies must be installed on the machine:
     - **Java**: Already installed during Jenkins setup.
     - **Maven**: Needed for build automation.
     - **Git**: Needed for interacting with GitHub.
   
   - To configure these tools in Jenkins, go to **Manage Jenkins > Tools** and configure Java, Maven, or Git.

2. **Configure Tools in Jenkins:**
   - You can install tools manually or let **Jenkins** handle the installation automatically.
   - Preferred option: Allow Jenkins to install Maven and Git automatically.

3. **Create Jenkins Job for Compilation:**
   - In Jenkins, create a new **Freestyle Project**.
   - **Source Code Management**: Choose **Git** and paste the **GitHub repository URL**.
   - **Build Step**: Choose **Invoke top-level Maven targets**.
   - **Define the Goal**: Specify the goal to be executed (e.g., `compile`).

4. **Define the Goal for Maven:**
   - The goal tells Maven what to do. For compiling Java code, use the goal `compile`.
   - **Maven** will read the `pom.xml` file, download dependencies, and compile the code.

5. **Running the Jenkins Job:**
   - Once the job is configured, click on **Build Now** to trigger the build process.
   - If the build fails, check the **Console Output** to debug the issue.

6. **Handling Errors with `pom.xml`:**
   - If there are multiple projects with separate `pom.xml` files, make sure Jenkins points to the correct `pom.xml` path.

---

## Jenkins Job Configuration Example

1. **Create a New Job:**
   - Name it (e.g., "My New Compile Job").
   - Select **Freestyle project** and click **OK**.
   - Add a description like "This is my new project."

2. **Configure GitHub Repository:**
   - Under **Source Code Management**, select **Git**.
   - Paste the **repository URL** from GitHub.
   - Choose the **branch** (e.g., `main`).

3. **Build Step - Use Maven:**
   - Choose **Invoke top-level Maven targets**.
   - Select the Maven version Jenkins has installed (e.g., `My Maven`).
   - Set the goal as **compile**.

4. **Run the Build:**
   - After saving the configuration, click **Build Now** to trigger the build.
   - If the build fails, check the **Console Output** for errors related to `pom.xml`.

---

## Fixing Common Errors

- If Jenkins fails to find the `pom.xml`, you may need to specify the correct location.
  - Go to **Job Configuration > Advanced > pom.xml Path** and specify the correct folder where the `pom.xml` file is located.
  
---

## Exploring Jenkins Workspace

- The **workspace** in Jenkins is where all files related to a job are stored. This includes the source code fetched from GitHub, the compiled code, and other job-specific files.
- Jenkins uses **Maven** to compile the code, fetch dependencies, and build the project.

---

## Automating with Jenkins Triggers

### Build Triggers in Jenkins

1. **Poll SCM (Source Code Management):**
   - This trigger monitors the source code repository (e.g., GitHub) and automatically triggers jobs when changes are detected.

2. **Build Periodically:**
   - Similar to **cron jobs** in Linux, you can schedule jobs to run at regular intervals. The cron-like syntax allows you to specify the frequency.
     - Example: `* * * * *` will trigger the job every minute.

### Example of Setting Build Triggers

- **Build Periodically**: Configure Jenkins to trigger builds at specific intervals.
  - Go to **Job Configuration > Build Triggers** and select **Build Periodically**.
  - Specify the cron-like syntax (e.g., `* * * * *` for every minute).

---

## Tool Installation and Configuration in Jenkins

- Jenkins allows you to install tools like **Java**, **Maven**, and **Git** either manually or automatically via **Global Tool Configuration**.
- **Global Tool Configuration** enables you to specify tools required for each job.

---

## Future Sessions

- In upcoming sessions, we will dive deeper into Jenkins with **Docker** and **Kubernetes** integration.
- We will cover more Jenkins topics like user administration, various triggers, and integrating with **YAML** files.

---

## Conclusion

- After today's session, you should have a basic understanding of how **Jenkins** and **Maven** work together to automate the build process.
- In tomorrow's session, we'll discuss **Build Triggers** and **Continuous Delivery (CD)**, and work on a real-world project with CI/CD, including Git deployment.

---

## Resources

- All session materials will be available on my **GitHub**.
- Search for **"Discover DevOps GitHub"** to find the resources, including AWS, S3, and more.
- The GitHub repository will be regularly updated with additional resources as we progress through the course.

---

