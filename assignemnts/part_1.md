# 🌐 Jenkins Assignment_1

 Tasks To Be Performed:
 1. Trigger a pipeline using Git when push on develop branch
 2. Pipeline should pull Git content to a folder

---
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

---

## 🚀 Launching 2 EC2 Instances
> Master and slave with same configuration 

### ☁️ EC2 Setup
- **Two Instances**:
  - `jenkins-master`
  - `jenkins-slave-1`

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

# 📘 GitHub + Jenkins Integration with Webhooks

## 🧩 Objective

To integrate **Jenkins** with **GitHub** using a **webhook** that triggers Jenkins pipelines when changes (like a push) are made to the `develop` branch in GitHub.

---

## 🗂️ Step-by-Step Breakdown

### ✅ Step 1: Setting Up Local Git Repo

1. **Create a Directory in Master Node**
   ```bash
   mkdir AWS1
   cd AWS1
   ```

2. **Initialize Git**
   ```bash
   git init
   ```

3. **Create a File for Initial Commit**
   ```bash
   touch master_file
   ```

4. **Stage and Commit the File**
   ```bash
   git add .
   git commit -m "master commit"
   ```

5. **Verify Git Status and Branch**
   ```bash
   git status
   git branch
   ```

6. **Create and Switch to `develop` Branch**
   ```bash
   git branch develop
   git checkout develop
   ```

7. **Create File for Develop Branch**
   ```bash
   touch develop_file
   git add develop_file
   git commit -m "commit for develop branch"
   ```

---

### 🔗 Step 2: Create GitHub Repository and Link to Local Git

1. **Create a New GitHub Repository**
   - Go to GitHub
   - Click on **New Repository**
   - Name it (e.g. `jenkins-aws`)
   - Keep it **Public**
   - Click **Create Repository**

2. **Add GitHub as Remote Origin**
   ```bash
   git remote add origin https://github.com/your-username/jenkins-aws.git
   ```

3. **Check Remote**
   ```bash
   git remote -v
   ```

4. **Push All Branches to GitHub**
   ```bash
   git push --all
   ```

> 📝 If asked for credentials, use a **Personal Access Token** instead of your GitHub password.

---

### 🔐 Step 3: Create GitHub Personal Access Token (PAT)

1. Go to **GitHub → Settings → Developer Settings**
2. Navigate to **Personal Access Tokens → Tokens (classic)**
3. Click on **Generate new token**
4. Provide:
   - Note: _e.g., Jenkins token_
   - Expiration: _90 days_
   - Scopes: _Check all necessary permissions_
5. **Copy and Save the Token** (you won't see it again!)

---

### 🔁 Step 4: Add Webhook in GitHub

1. Go to your repository → **Settings → Webhooks**
2. Click on **Add Webhook**
3. Enter the Payload URL:
   ```
   http://<your-jenkins-ip>:8080/github-webhook/
   ```
4. Content Type: `application/json`
5. Select **Just the push event**
6. Click **Add Webhook**

✅ You'll see a green checkmark if the webhook is correctly configured.

---

## 📦 Final Repo Structure

```
jenkins-aws/
├── master_file        # For master branch
└── develop_file       # For develop branch
```

---

## 🧪 Validate Webhook

- Make a change in the `develop` branch (e.g., edit `develop_file`)
- Push to GitHub:
  ```bash
  git add .
  git commit -m "test webhook"
  git push origin develop
  ```
- Jenkins pipeline should be triggered (if configured to listen to GitHub webhooks).

---

## 📌 Notes

- `ls -la` is used to show long list format with hidden files.
- `rwx` permissions:
  - `r`: read
  - `w`: write
  - `x`: execute
- `git switch <branch>` is a modern alternative to `git checkout <branch>`

---

# 📘 Jenkins Assignment #1 – Freestyle Job with Git Integration

This assignment involves creating a **freestyle project** (Job) in Jenkins, pulling code from a GitHub repository, and verifying its execution on a **slave node** (agent) instead of the master.

---

## 🧱 Objective

- Create a **freestyle Jenkins job**.
- Integrate it with a **GitHub repository**.
- Configure it to run on a **Jenkins slave node**.
- Ensure the build creates a **workspace directory** on the slave with the correct content.

---

## 🧰 Prerequisites

- Jenkins is installed and running.
- At least one **slave node** (agent) is configured.
- A **GitHub repository** is available with the project files.
- SSH credentials or GitHub token are set up in Jenkins.

---

## 📝 Step-by-Step Instructions

### 1. Go to Jenkins Dashboard

- Visit your Jenkins web UI.
- On the top left, click **"New Item"**.

---

### 2. Create a Freestyle Project

- Enter a name, e.g., `job1`.
- Select **Freestyle Project**.
- Click **OK**.

---

### 3. General Configuration

- (Optional) Check **GitHub project** and enter the **GitHub repo URL**.
- Check **Restrict where this project can be run**.
  - Enter the **label** of the slave node (e.g., `node1`).

---

### 4. Source Code Management

- Select **Git**.
- Enter the Git repository URL.
- Add/select credentials (e.g., SSH key or token).
- In **Branch to build**, use:  
  ```
  */develop
  ```
  _(As per assignment requirement.)_

---

### 5. Build Triggers

- Select:
  - **GitHub hook trigger for GITScm polling**  
    _(Triggers build on GitHub webhook push.)_

---

### 6. Save and Build

- Click **Save**.
- Go to the job dashboard.
- Click **"Build Now"**.
- Confirm that a build is triggered.

---

## 🖥️ Verifying on Slave Node

Once the job is executed:

1. **SSH into the slave machine**.
2. Navigate to Jenkins home directory:
   ```bash
   cd ~/Jenkins
   ```
3. Check for `workspace` directory:
   ```bash
   ls
   ```
   You should see a `workspace/` folder now.

4. Go inside:
   ```bash
   cd workspace/job1
   ls
   ```
   You should see the files from your GitHub repo (from `develop` branch) here.

---

## 📂 Extra Debugging Info

If you do **not** see the workspace:

- Make sure the job is **restricted to the correct slave node** under "Restrict where this project can be run".
- Ensure the **build is completed successfully**.
- Rebuild after correcting any misconfigurations.

---

## 📎 Notes

- Files like `.git` and temporary `.tmp` files might appear in the directory — these are expected.
- Use `ls -la` to see hidden and system files.

---

## ✅ Conclusion

You’ve now successfully:

- Created a Jenkins freestyle job.
- Integrated it with GitHub.
- Ran it on a slave node.
- Verified the presence of build output in the Jenkins workspace on the slave.

---

