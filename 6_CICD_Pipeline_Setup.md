# CI/CD Pipeline Setup and Jenkins Master-Slave Architecture

## Recap of the Previous Work

### CI/CD Pipeline Creation

- **Initial Setup**: We began by setting up a Continuous Integration/Continuous Deployment (CI/CD) pipeline. The process starts when a developer writes code and pushes it to a GitHub repository.
- **Jenkins Setup**: Jenkins is configured on a Linux server, and we created two key jobs: **Compile** and **Test**.
  - **Compile Job Configuration**:
    - The GitHub repository is linked to Jenkins as the source of the code.
    - Build triggers like **GitHub Webhook** are used to automatically trigger jobs when new commits are pushed. Other options such as **Build periodically** and **Poll SCM** are also available, but GitHub Webhook is the best choice for automatic triggering.
  
### Test Job Configuration
- **Sequential Execution**: The **Test Job** is set to run only after the **Compile Job** completes successfully. 
- **Pipeline Structure**: The jobs are organized in a pipeline:
  - **Compile → Test → Package**.
- **Upstream and Downstream Jobs**:
  - **Compile** has no upstream jobs but has downstream jobs like **Test** and **Package**.
  - **Test** depends on **Compile** and triggers **Package** as its downstream job.

### Managing Multiple Teams and Machines
- **Resource Management**: Multiple teams might want to run their jobs on different machines, especially when the Jenkins server becomes resource-constrained (e.g., CPU/RAM limitations).
- **Master-Slave Architecture**: We implemented a master-slave setup where the **Master server** controls the job triggers, and the **Slave server** runs the jobs.

### SSH Key Setup for Passwordless Connection
- **Passwordless SSH Setup**: We configured passwordless SSH access between the master and slave machines to enable seamless communication between them.
  - **Generate SSH Key**: Use `ssh-keygen` to create an SSH key pair (public and private keys).
  - **Copy the Public Key**: Use `cat` or `ssh-copy-id` to copy the public key from the master to the slave, placing it in the slave's `~/.ssh/authorized_keys` file.
  - **Test SSH Connection**: Ensure passwordless SSH is working by running `ssh slave-ip` from the master.

### Jenkins Plugin Updates
- **Plugin Maintenance**: We discussed the importance of updating Jenkins plugins, but also ensuring the updates are tested on a development server before being applied to production.

### Pipeline as Code
- **Declarative Pipelines**: In a real-world scenario, it is better to define Jenkins pipelines as code, typically in YAML or similar formats. This ensures the job configuration is codified and easily version-controlled.
  
---

## Master-Slave Architecture

### Understanding Master-Slave Setup
- **Master Server**: The master Jenkins server controls the jobs, triggering them based on conditions and available resources.
- **Slave Server**: The slave executes the jobs assigned by the master. This helps offload the resource burden from the master.

### Passwordless SSH Setup for Master-Slave Connection
1. **Generate SSH Keys** on the master.
2. **Copy the public key** to the slave’s `~/.ssh/authorized_keys` file.
3. **Verify SSH Authentication** by running `ssh slave-ip` from the master.

### Adding Slave Node in Jenkins
1. **Access Jenkins Dashboard**: Navigate to **Manage Jenkins → Manage Nodes**.
2. **Create a New Slave Node**:
   - Name the node (e.g., "MySlave").
   - Configure the number of executors (set to 1 for this example).
   - Set the remote workspace directory (e.g., `/tmp` for testing purposes).
   - Add a label to identify the slave (e.g., "MySlave").
   - Select **Launch agent via SSH** as the launch method and provide the slave’s private IP.
   
3. **Test Connection**: After saving the configuration, Jenkins should show the slave as **online**. If it doesn't, troubleshoot by verifying the SSH connection and Java installation on the slave.

### Java Version Compatibility
- **Issue**: The Jenkins master was running **Java 17**, while the slave was running **Java 11**.
- **Solution**: Install Java 17 on the slave and reboot the system.
- **Result**: The slave agent successfully connected after resolving the Java version mismatch.

---

## Running Jobs on Slave Server

### Configuring Jobs to Run on Slave
1. **Assign a Job to a Specific Slave**:
   - When creating or configuring a job, you can specify the slave on which the job should run. This is done by selecting the label assigned to the slave (e.g., "MySlave").
   
2. **Job Execution**:
   - Once the job is configured, it will be executed on the slave, and the workspace will be created on the slave machine (e.g., under `/tmp`).
   - You can verify this by checking the **Console Output** in Jenkins, which will indicate that the job is running on the slave.

### Workspace Setup
- The workspace for jobs running on the slave will be created in the specified directory, such as `/tmp/workspace` on the slave machine. This ensures the job is executed remotely on the slave instead of the master server.

### Using Public and Private SSH Keys
- **Public Key**: The public key is placed on the slave machine to enable secure communication from the master.
- **Private Key**: The private key remains on the master machine, ensuring that only the master can authenticate to the slave.

---

## Session Wrap-Up

- **Review**: We have covered the basics of Jenkins setup, including the creation of jobs, master-slave architecture, SSH key configuration, and managing resources across multiple machines.
- **Next Steps**: On **Tuesday**, we will continue hands-on work, including:
  - CI/CD Jobs.
  - Jenkins administration tasks (e.g., user management).
  - Introduction to **Docker** and **Kubernetes**.

---

## Rest and Final Thoughts
- **Instructor's Rest**: After a long session, the instructor needs some rest but assures the class that the next session will be even more engaging and filled with energy.
- **Stay Tuned**: On **Tuesday**, we will dive deeper into CI/CD pipelines, explore Jenkins in more detail, and introduce Docker and Kubernetes concepts for containerization and orchestration.

---

### Key Concepts:
- **Master-Slave Architecture**: Ensures Jenkins can scale and manage multiple teams and jobs.
- **Passwordless SSH**: Facilitates seamless communication between the master and slave without requiring password authentication.
- **Job Execution on Slave**: Allows offloading resource-heavy tasks from the master to a dedicated slave machine.
- **Workspace**: Defines the location where job files and data are stored during execution.

