# 📘 Assignment 2: Jenkins - Add Node & Create Jobs (`push-to-test` and `push-to-prod`)

## 🔧 Objective
Create a Jenkins setup with:
- One Master Node (already exists)
- Two Slave Nodes (Node1 and Node2)
- Two Jenkins Jobs: `push-to-test` and `push-to-prod`
- Trigger builds based on GitHub branch pushes (to `test` and `master` respectively)

---

## 🖥️ Step 1: Add One More Slave Node (`Node2`)

### ✅ Actions:
1. **Launch EC2 Instance** (for second slave):
   - Name: `Jenkins Slave 2`
   - Type: `t2.micro`
   - AMI: Ubuntu (e.g., Ubuntu 22.04)
   - Key: Use your existing key pair
   - Security Group: Allow SSH (Port 22)
2. **Install Java** on the new instance:
   ```bash
   sudo apt update
   sudo apt install openjdk-17-jdk -y
   java -version
   ```

---

## ⚙️ Step 2: Add Slave Node in Jenkins (`Node2`)

### 🔧 Jenkins UI Path:
> Manage Jenkins → Nodes → New Node → Copy from Node1 or Create New

### ⚙️ Configuration:
- **Node Name**: `Node2`
- **Remote root directory**: `/home/ubuntu/jenkins`
- **Launch method**: SSH
- **Host**: Use the **Private IP** of the second EC2 instance
- **Credentials**: Use existing SSH credentials (e.g., `ubuntu` user)
- **Host Key Verification Strategy**: `Non verifying verification strategy`
- ✅ Save the node and ensure status is `Online`

---

## 🛠️ Step 3: Create New Job - `push-to-prod`

### 🔧 Jenkins UI Path:
> Jenkins Dashboard → New Item → Freestyle Project

- **Job Name**: `push-to-prod`
- **Copy from existing job**: `push-to-test` (if exists)

### 📝 Configuration:

#### General:
- GitHub Project URL (your repo)

#### Restrict where this project can run:
- **Node Label**: `Node2`

#### Source Code Management:
- **Git**: Add GitHub repo URL
- **Branch to build**: `test`

#### Build Triggers:
- ✅ GitHub hook trigger for GITScm polling

---

## 🌿 Step 4: Create `test` Branch in GitHub Repo

### ✅ Actions on Jenkins Master Terminal:
```bash
git checkout -b test          # create and switch to test branch
touch test_file.txt
git add test_file.txt
git commit -m "test file added"
git push origin test          # Important! This pushes to GitHub
```

> 🔎 Confirm that `test` branch appears on GitHub

---

## ▶️ Step 5: Run Job and Validate

### 💥 Run the Job:
- From Jenkins Dashboard, build `push-to-prod` job manually (or push to test to auto-trigger)

### 🧪 Validate:
- SSH into **Slave Node 2**
- Check workspace directory:
```bash
cd /home/ubuntu/jenkins/workspace/push-to-prod
ls
```

✅ You should see the files from the `test` branch (`test_file.txt`)

---

## ❗ Troubleshooting Tips

### 🛠 Common Issues:
- **“Couldn’t find any revision to build”**:
  > Reason: You didn’t push the test branch to GitHub yet.

- **Node offline or job doesn’t run**:
  > Check IP, Java installed, and SSH access from Jenkins master.

---

## ✅ Summary

| Component        | Value                     |
|------------------|---------------------------|
| Jenkins Master   | 1 Instance                |
| Slave Nodes      | Node1, Node2              |
| Jobs Created     | `push-to-test`, `push-to-prod` |
| Branches Used    | `develop`, `test`, `master`     |
| Trigger Type     | GitHub webhook            |

---
