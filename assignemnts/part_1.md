# 🌐 Jenkins Assignment_1

 We will:
- Launch two EC2 instances (one **Master** and one **Slave**)
- Install Jenkins
- Set up and trigger a CI/CD pipeline
- Use a GitHub repository (triggered by a push to the `develop` branch)

---

## 📝 Assignment Overview

### 📌 Task Breakdown
1. **Trigger a pipeline** on **push to the `develop` branch`**.
2. **Pipeline pulls GitHub content** into a specified folder/directory.

🛠️ CI/CD Pipeline Flow Diagram

               ┌────────────────────────────┐
               │ Developer pushes code to   │
               │    'develop' branch        │
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ GitHub triggers Webhook    │
               │  to notify Jenkins         │
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ Jenkins receives the event │
               │ (push on develop)          │
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ Jenkins Pipeline Job       │
               │ starts automatically       │
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ Pull source code from      │
               │ GitHub repo (develop)      │
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ Clone/download code to     │
               │ a specified directory      │
               │ (e.g., /var/lib/jenkins/...│
               └────────────┬───────────────┘
                            │
                            ▼
               ┌────────────────────────────┐
               │ Next steps in pipeline     │
               │ (e.g., Build, Test, Deploy)│
               └────────────────────────────┘

---

## 🚀 Launching 2 EC2 Instances
> Master and slave with same configuration 

### ☁️ EC2 Setup
- **Two Instances**:
  - `jenkins-master`
  - `jenkins-slave-1`
- **AMI**: Ubuntu 22.04 (more stable than 24.04)
- **Instance Type**: `t2.micro`
- **Security Group**: Allow **All Traffic** (Inbound rule: `Anywhere (0.0.0.0/0)`)
- **Storage Configuration**: default

> ⚠️ **Important**: Refresh EC2 dashboard until `2/2 status checks passed` before connecting.

---

## 🔐 Instance Access

### ✅ Connect and Update Instances

```bash
# First command after connecting:
sudo apt update
```

Use `sudo su` if you want to switch to root.

---

## ☕ Installing Java (JDK 17)

### 📍 Install on Both Master and Slave

```bash
sudo apt install openjdk-17-jdk -y
```

### ✅ Verify Java Installation

```bash
java -version
```

Expected Output:
```
openjdk version "17.0.x"
```

---

## 🏗️ Installing Jenkins (on Master only)

### 📂 Official LTS (Long Term Support) Commands

Get from [Jenkins Official Website](https://www.jenkins.io/download/)

### 🖥️ Create Jenkins Installation Script

```bash
sudo nano jenkins_install.sh
```

**Paste the following LTS install commands** inside the file, then save and exit using `Ctrl+S`, `Ctrl+X`.

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```

### 🏃 Run the Script

```bash
bash jenkins_install.sh
```

### ✅ Check Jenkins Status

```bash
sudo service jenkins status
```

Expected: `active (running)`

---

## 🌍 Access Jenkins Dashboard

### 🔗 In Browser

```
http://<EC2-PUBLIC-IP>:8080
```

> Port `8080` is Jenkins default.

---

## 🔓 Unlock Jenkins

### 📂 Get Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### ✍️ Paste in Dashboard and Proceed

---

## 🧩 Install Suggested Plugins

Click:
> **Install suggested plugins**

✅ Wait for the following plugins:
- Git
- Pipeline
- Gradle
- LDAP
- Many more...

---

## 👤 Create First Admin User

Fill out the form:
- **Username**: paric
- **Password**: [YourPassword]
- **Full Name**: Paric
- **Email**: your_email@domain.com

Click:
> **Save and Continue**

---

## ✅ Finalize Setup

- Confirm Jenkins is accessible at `http://<IP>:8080`
- Click: **Start using Jenkins**

---
## 🖥️ Jenkins Dashboard Overview

Once setup is complete, you’ll land on the Jenkins dashboard.

### Main Sections:

- **New Item**: Create new jobs/pipelines
- **Build History**: Past builds list
- **Manage Jenkins**: Admin settings
- **Build Queue**: Active or queued jobs
- **User Info**: Top-right corner

---

## Configure Jenkins Settings

Navigate to:  
**Manage Jenkins → Security → Configure Global Security**

### Key Change:

- Scroll to **"TCP Port for Inbound Agents"**
> Here the Inbound Agents means Whosoever will be trying to access the jenkins are inbound agents (= Nodes).
> So we have to do it to random in order to create a node
> - because if you will not do this, when you will create a node, it'll give you a red color of cross, which means that will be an offline state.
> - So in order to get thet node in online status, we have to do this to random.

- Change from:
  ```plaintext
  ⭕ Disable
  ```
  To:
  ```plaintext
  🔘 Random
  ```

This allows Jenkins agents (slaves) to connect.

Save the configuration.

---

# 🔌 Jenkins Setup – Plugin Configuration

---

## 🔌 Plugin Installation – SSH Agent Plugin

To create **nodes (agents)** in Jenkins, we need to install a **plugin** called:

### ✅ SSH Agent Plugin

---

### 📍 Step-by-Step Installation:

1. Navigate to:  
   **Manage Jenkins → Plugin Manager**

2. Click on the **Available** tab to search for new plugins.

3. In the search bar, type:

   ```plaintext
   SSH Agent
   ```

4. Locate the **SSH Agent Plugin** from the list.

5. Check the box next to it and click:

---

### 🟢 Plugin Installation Output

- Jenkins shows the progress of installation.
- You’ll see a `Success` message indicating the plugin is successfully installed.
- If other plugins fail (e.g., dark theme plugin), that’s fine — they are optional.

---

### ✅ Verify Plugin Installation

1. Go back to:  
   **Manage Jenkins → Plugin Manager**

2. Click the **Installed** tab.

3. Search for:

   ```plaintext
   SSH Agent
   ```

4. You should see it listed with status **enabled**.

---

### 🔧 Plugin Management Options

From the Installed tab, you can:
- **Disable** a plugin temporarily
- **Uninstall** it if it's no longer needed

> ⚠️ Do not uninstall or disable **SSH Agent Plugin**, as it's essential for connecting nodes (agents) to the Jenkins master.

---

# 🖥️ Jenkins – Create and Configure a Node (Agent)

In Jenkins, a **Node** (or agent) refers to a separate machine or instance used to run jobs independently from the master (controller). This setup helps in distributing the workload and managing builds efficiently.

---

## 🚀 Step-by-Step Guide: Create a Jenkins Node (Permanent Agent)

---

### 🔹 Step 1: Go to "Manage Jenkins"

- From the Jenkins **Dashboard**, navigate to:
  ```text
  Manage Jenkins → Nodes 
  ```

- You will see a **built-in node** listed, which is Jenkins' own controller node.

---

### 🔹 Step 2: Create a New Node

1. Click on `New Node`.
2. Enter a **node name** – for example: `node1`.
3. Choose **Permanent Agent** as the node type.
   - 📌 *Permanent agents* are machines that are permanently attached to Jenkins and run jobs as assigned.
   - Jenkins doesn’t provide high-level integration natively; this is a manual integration method.
4. Click **OK** to proceed to configuration.

---

### 🔹 Step 3: Node Configuration

Here's the full configuration breakdown:

| Field                     | Value / Description                                                                 |
|--------------------------|--------------------------------------------------------------------------------------|
| **Description**          | *(Optional)* e.g., `This node is for Assignment 1`                                  |
| **# of Executors**       | 1 (or as needed – defines how many jobs this node can run concurrently)              |
| **Remote root directory**| `/home/ubuntu/jenkins/` – Path to Jenkins home on the agent system                  |
| **Usage**                | "Use this node as much as possible"                                                 |
| **Launch method**        | `Launch agents via SSH`                                                             |
| **Host**                 | Private IP of your slave machine (e.g., AWS EC2 instance)                           |

---

### 🔹 Step 4: Add SSH Credentials

Since we are connecting via SSH, we need to add proper credentials.

1. Click on `Add → Jenkins` under **Credentials**.
2. Select the following:

   | Field         | Value |
   |---------------|-------|
   | **Kind**      | SSH Username with Private Key |
   | **Username**  | `ubuntu` (for Ubuntu AMI) |
   | **Private Key** | Select: `Enter directly`, then paste your `.pem` key contents |
   | **Description** | *(Optional)* For identification |

3. Save the credential.

✅ Once added, select the newly added credential (e.g., “ubuntu”) from the dropdown.

---

### 🔹 Step 5: Host Key Verification Strategy

- Select:  
  ```text
  Non verifying verification strategy
  ```

This avoids failures due to unknown host key issues, especially in dynamic environments like cloud VMs.

---

### 🔹 Step 6: Final Settings

- **Availability**:  
  Select:  
  ```text
  Keep this agent online as much as possible
  ```

- Click **Save** to complete the node creation.

---

## ✅ Verify Node Status

1. After saving, Jenkins attempts to connect to the node.
2. The node will appear in the dashboard under the list of agents.
3. Look for:
   - **Online** status ✔️
   - Green indicator next to the node name
   - If any issue occurs, it will show a red ❌ or say “Offline”

4. You can manually refresh by clicking `Refresh Status`.

---

## 🟢 Success!

Your node (`node1`) is now **successfully created and online**. Jenkins can now run jobs on this agent.

---

## 📌 Notes

- Make sure the **.pem** file is correct and accessible.
- The remote machine (agent) should have **Java installed** and the right **permissions set**.
- If a node goes **Offline**, check the logs for connection or key issues.

---


