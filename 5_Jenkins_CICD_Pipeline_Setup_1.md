# Jenkins CI/CD Pipeline Setup Documentation

## 1. Job Configuration

### 1.1. Source Code Management (SCM)
- **Purpose**: In Jenkins, we configure jobs to automatically pull the latest version of the code from a version control system, such as Git.
- **Steps**:
    1. **Go to Jenkins Dashboard** and select the job to configure.
    2. **Under the "Source Code Management" section**, select **Git**.
    3. Provide the **Git Repository URL**.
    4. Specify the **Branch** you want Jenkins to work with (e.g., `main` or `dev`).

This ensures that Jenkins always uses the latest code from the specified branch for building and testing.

<img width="959" alt="image" src="https://github.com/user-attachments/assets/d0680211-cd2a-47bf-b22a-9b526ffc56f5" />


### 1.2. Build Step
- **Purpose**: The build step is where Jenkins compiles the code and creates a build artifact.
- **Steps**:
    1. In the **"Build"** section, choose **"Invoke top-level Maven targets"** if you are using Maven for building the project.
    2. Specify the **Goals** (e.g., `clean install`) for Maven.
    3. If needed, install **Maven** and **Java** tools on Jenkins either manually or using automatic installation.

---

## 2. Build Triggers

### 2.1. Build Periodically
- **Purpose**: Runs the job on a schedule.
- **Steps**:
    1. Go to the **"Build Triggers"** section.
    2. Check the **"Build Periodically"** option.
    3. Set the schedule in **cron format** (e.g., `0 16 * * *` runs every day at 4:00 PM).

### 2.2. Poll SCM
- **Purpose**: Triggers the job when there is a change (commit) in the repository.
- **Steps**:
    1. Under **"Build Triggers"**, check the **"Poll SCM"** option.
    2. Specify the interval to check for changes, e.g., `H/5 * * * *` checks every 5 minutes.

### 2.3. GitHub Webhook
- **Purpose**: Automatically triggers the Jenkins job when new code is pushed to GitHub.
- **Steps**:
    1. **Jenkins Configuration**:
        - Go to Jenkins, select your project, and click on **Configure**.
        - Under **"Build Triggers"**, uncheck **Poll SCM** and check **"GitHub hook trigger for GITScm polling"**.
        - Save the configuration.
    2. **GitHub Webhook Setup**:
        - Go to your GitHub repository, navigate to **Settings > Webhooks**.
        - Click **Add Webhook** and in the **"Payload URL"**, add `http://<jenkins-ip>:8080/github-webhook/`.
        - Set the **Content type** to `application/json` and save.

By following this setup, Jenkins will trigger a build every time a new commit is pushed to your GitHub repository.

---

## 3. Testing the Webhook

### 3.1. Test Webhook Functionality
- **Steps**:
    1. Make a change to the GitHub repository (e.g., edit the README file) and **commit** the change.
    2. Go to Jenkins, refresh the page, and you should see the job triggered by the commit.
    3. You can repeat the process by making additional changes to verify the webhook trigger.

### 3.2. Why Webhook is Better
- **GitHub Webhook** ensures that the job runs **immediately** after a commit, unlike the other triggers, which might delay the execution. This provides real-time automation without manual intervention.

---

## 4. Test Job Configuration

### 4.1. Triggering the Test Job
- **Steps**:
    1. Go to the **Test Job** and click on **Configure**.
    2. In the **"Build Triggers"** section, select **"Build after other projects"**.
    3. Select **Compile Job** as the job to trigger the Test Job once it finishes.
    4. Save the configuration.

### 4.2. Testing the Workflow
- After configuring the Test Job, the test job will only run after the **Compile Job** finishes. Test it by committing a change to GitHub, which will trigger the **Compile Job**. Once it finishes, the **Test Job** will start.

---

## 5. Pipeline Concept

### 5.1. CI/CD Pipeline Overview
- This setup automates the build and test process:
    - A **commit** in GitHub triggers the **Compile Job**.
    - Once the **Compile Job** finishes, the **Test Job** is triggered.
- This represents a basic **Continuous Integration (CI)** setup in Jenkins.

---

## 6. Jenkins Master-Slave Architecture

### 6.1. Why Use Master-Slave Architecture?
- The **Master-Slave** architecture helps distribute the workload across multiple machines:
    - **Master** handles job configuration and scheduling.
    - **Slave** machines execute the jobs, preventing overloading the master server.

### 6.2. Setting Up Master-Slave
- **Steps**:
    1. Set up passwordless SSH between the **Master** and **Slave** machines.
        - On the master, generate SSH keys with `ssh-keygen -t rsa`.
        - Copy the **public key** to the **Slave** machine using `ssh-copy-id`.
    2. After setting up SSH, the master can send jobs to the slave without entering a password.
    3. **Install tools** (e.g., Java, Maven) on the slave machine as needed for your jobs.

---

## 7. Jenkins Plugins

### 7.1. Installing Plugins
- **Steps**:
    1. Go to **Manage Jenkins > Manage Plugins**.
    2. Search for the required plugin (e.g., **Build Pipeline View**) and click **Install**.

### 7.2. Using Plugins
- After installing, you can access new functionalities. For example, the **Build Pipeline View** plugin allows you to visualize job pipelines.
    1. Go to **Dashboard > Build Pipeline View**.
    2. Set up the pipeline with the **Compile Job** as the first job.
    3. Jenkins will now show the status of jobs in the pipeline visually.

---

## 8. Jenkins Dashboard Views

### 8.1. Monitoring Jobs
- Dashboards provide a visual representation of job statuses:
    - **Yellow** indicates the job is running.
    - **Blue** means the job is preparing.
    - **Green** indicates success, while **Red** indicates failure.

---

## 9. Next Steps

- **Master-Slave Setup**: 
    - We'll continue to configure the slave and run jobs on it.
- **Continuous Delivery (CD)**: 
    - Configure a **CD** pipeline to automate deployments after successful tests.

---

By following these steps, you now have a fully automated Jenkins CI/CD pipeline setup, capable of handling tasks like building, testing, and visualizing job statuses. The integration with GitHub webhooks allows immediate triggering of Jenkins jobs whenever a new commit is made, ensuring a continuous and efficient development workflow.
