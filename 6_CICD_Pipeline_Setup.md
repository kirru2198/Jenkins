# CI/CD Pipeline Setup and Jenkins Master-Slave Architecture
  
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

---

### Pipeline as Code
- **Declarative Pipelines**: In a real-world scenario, it is better to define Jenkins pipelines as code, typically in YAML or similar formats. This ensures the job configuration is codified and easily version-controlled.

---

### Adding Slave Node in Jenkins
1. **Access Jenkins Dashboard**: Navigate to **Manage Jenkins → Manage Nodes**.
2. **Create a New Slave Node**:
   - Name the node (e.g., "MySlave").
   - Configure the number of executors (set to 1 for this example).
   - Set the remote workspace directory (e.g., `/tmp` for testing purposes).
   - Add a label to identify the slave (e.g., "MySlave").
   - Select **Launch agent via SSH** as the launch method and provide the slave’s private IP.
   
3. **Test Connection**: After saving the configuration, Jenkins should show the slave as **online**. If it doesn't, troubleshoot by verifying the SSH connection and Java installation on the slave.

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



