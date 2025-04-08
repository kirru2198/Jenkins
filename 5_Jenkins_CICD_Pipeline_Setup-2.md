# Jenkins CI/CD Pipeline Setup

## Overview
In this document, we will discuss the configuration of a Jenkins CI/CD pipeline, which includes setting up jobs for compiling and testing code, configuring build triggers, and utilizing GitHub webhooks for automation.

---

## Job Configuration

### 1. **Source Code Management (SCM)**

- **Description**: The source code management (SCM) section in Jenkins allows us to link Jenkins to a version control system, like Git.
- **Steps**:
  - Choose **Git** as the SCM tool.
  - Provide the **Git repository URL** and specify the **branch** to work with.
  - This ensures Jenkins pulls the correct code each time a job is triggered.

```markdown
Example SCM Configuration:
- Repository URL: `https://github.com/user/project.git`
- Branch: `main`
```

---

### 2. **Build Step (Maven)**

- **Description**: In the build step, Jenkins uses Maven (a build tool) to compile the code.
- **Steps**:
  - Install necessary tools like **Maven** and **Java** on Jenkins.
  - Configure Jenkins to use Maven to build the project during the pipeline execution.

```markdown
Example Build Step:
- Tool: Maven
- Goal: `clean install`
```

---

## Build Triggers

### 1. **Build Periodically**

- **Description**: This trigger allows Jenkins to run the job on a fixed schedule using a cron-like syntax.
- **Example**: `0 16 * * *` means the job runs every day at 4:00 PM.
  
  ```markdown
  Cron Syntax Explanation:
  - `0`: minute
  - `16`: hour
  - `*`: day of month
  - `*`: month
  - `*`: day of week
  ```

### 2. **Poll SCM**

- **Description**: Poll SCM checks for changes in the repository at regular intervals. The job is triggered only when changes are detected.
  
  ```markdown
  Example Polling Configuration:
  - Poll SCM every 5 minutes: `H/5 * * * *`
  ```

### 3. **Differences Between Build Periodically and Poll SCM**

- **Build Periodically**: Runs the job based on a fixed schedule, regardless of whether the code has changed.
- **Poll SCM**: Runs the job only when a change (commit) is detected in the repository.

---

## Ideal Scenario: Using GitHub Webhooks

- **Why GitHub Webhooks**: Webhooks provide a real-time method to trigger Jenkins jobs when code changes occur in GitHub, avoiding the delay of scheduled or periodic builds.

### 1. **Jenkins Configuration for GitHub Webhook**
   - Go to Jenkins, select your project, and click on **Configure**.
   - Under **Build Triggers**, uncheck **Poll SCM** and check **GitHub hook trigger for GITScm polling**.
   - Save the configuration.

### 2. **GitHub Webhook Setup**

- Navigate to your GitHub repository settings.
- Click on **Webhooks**, then **Add Webhook**.
- In the **Payload URL** field, enter `http://<jenkins-ip>:8080/github-webhook/`.
- Set the **Content type** to `application/json`.
- Save the webhook.

### 3. **Handling AWS Public IP**  
  If using AWS, you can assign an **Elastic IP** (static IP) or use a DNS name for the Jenkins server to avoid the issue of IP address changes.

---

## Testing GitHub Webhook

1. After configuring the GitHub webhook, test it by making a change in the GitHub repository.
2. For example, modify the **README** file and commit the change.
3. Refresh Jenkins to see if the job has triggered.

---

## Test Job Configuration

### 1. **When Should the Test Job Run?**

- **Description**: The test job should run **only after the compile job** finishes.
  
### 2. **Configuring the Test Job**
   - Go to the **Test Job** in Jenkins and click **Configure**.
   - Under **Build Triggers**, select **Build after other projects**.
   - Choose the **Compile Job** as the job that should trigger the test job.

### 3. **Testing the Workflow**
   - When a commit triggers the **Compile Job**, the **Test Job** will automatically run once the compile job completes.

---

## Pipeline Concept

### 1. **Automated Pipeline Flow**

- A commit in GitHub triggers the **Compile Job**.
- After the compile job finishes, it automatically triggers the **Test Job**.
- This forms a Continuous Integration (CI) pipeline.

---

## Jenkins Job Dependencies: Upstream and Downstream

### 1. **Upstream and Downstream Jobs**
   - **Upstream**: A job that triggers another job (e.g., **Compile Job** triggers **Test Job**).
   - **Downstream**: A job that depends on the completion of another job (e.g., **Test Job** depends on **Compile Job**).

### 2. **Job Behavior**
   - The **Test Job** will only run after the **Compile Job** completes, based on the configuration under **Build Triggers**.

---

## Managing Jenkins Plugins

### 1. **Installing Plugins**
   - Navigate to **Manage Jenkins > Manage Plugins**.
   - Search for and install plugins, like **Build Pipeline View** to visualize Jenkins jobs in a pipeline.

### 2. **Using Installed Plugins**
   - Once installed, you can use the **Build Pipeline View** to create a visual pipeline, making it easier to track job statuses.

---

## Jenkins Master-Slave Architecture

### 1. **Why Use Master-Slave Architecture?**
   - The **Master** handles configuration and job scheduling.
   - The **Slave** machine executes the jobs, preventing performance bottlenecks on the master server.

### 2. **Setting Up Passwordless SSH for Master-Slave**
   - Generate SSH keys on the master machine using `ssh-keygen`.
   - Copy the **public key** (`id_rsa.pub`) to the **slave machine** to enable passwordless SSH.
   - Test the connection with `ssh user@slave_ip`.

---

## Example SSH Commands

### 1. **SSH Key Generation on Master**
```bash
ssh-keygen -t rsa
```
   - This generates `id_rsa` (private key) and `id_rsa.pub` (public key).

### 2. **Copy Public Key to Slave**
```bash
ssh-copy-id user@slave_ip
```

### 3. **Testing SSH Connection**
```bash
ssh user@slave_ip
```

---

## Next Steps

1. We will finish setting up the slave machine and configure **Continuous Delivery (CD)** to deploy code to production environments.
2. We'll also configure a **Jenkins Slave** machine for additional job execution, ensuring better performance and scalability.

