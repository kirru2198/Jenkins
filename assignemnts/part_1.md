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

