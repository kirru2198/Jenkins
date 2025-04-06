Hands-On: Jenkins

1. Create 3 instances (Master, Slave-1, Slave-2) on EC2 server.
2. Install Jenkins on Master. (Refer to the Jenkins installation documentation)
3. Set up a Jenkins Master-Slave Cluster on AWS
4. Create a CI/CD pipeline triggered by Git Webhooks.
 
First, we have created 3 instances: Master (Green terminal), Slave-1 (Orange Terminal) and Slave-2 (Blue Terminal) on EC2 Server. And then we have installed Jenkins on the Master Machine. Now Let us set up the Jenkins Master-Slave Cluster.

---

## Hands-On: Jenkins

### Step 1: Create 3 EC2 Instances
1. **Create 3 instances on EC2**: 
   - **Master** (Green terminal)
   - **Slave-1** (Orange Terminal)
   - **Slave-2** (Blue Terminal)

2. **Install Jenkins on Master**:
   - SSH into the Master instance and run the following commands to install Jenkins:
     ```bash
     sudo apt update
     sudo apt install openjdk-11-jdk
     wget -q -O - https://pkg.jenkins.io/keys/jenkins.io.key | sudo apt-key add -
     echo "deb http://pkg.jenkins.io/debian/ stable main" | sudo tee -a /etc/apt/sources.list.d/jenkins.list
     sudo apt update
     sudo apt install jenkins
     sudo systemctl start jenkins
     sudo systemctl enable jenkins
     ```

3. **Expected Output**: 
   - After installation, Jenkins will be running on port `8080`. To confirm Jenkins is up and running, use:
     ```bash
     sudo systemctl status jenkins
     ```
     Expected output: 
     ```
     ● jenkins.service - Jenkins
        Loaded: loaded (/etc/systemd/system/jenkins.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2025-04-06 12:00:00 UTC; 1min 30s ago
        ...
     ```

### Step 2: Configure Jenkins
1. **Access Jenkins**: Open a browser and navigate to `http://<Master_IP>:8080`.

2. **Unlock Jenkins**: Copy the unlock key using the command:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
   Expected output: 
   ```
   6c7a7e04d6bb43a4a1f3038fd8e7fd50
   ```

3. Paste this password on the Jenkins setup page and click **Continue**.

4. **Install Suggested Plugins**: Choose "Install Suggested Plugins" to install the necessary plugins.

5. **Set Admin Details**: Create an admin user with a username and password.

6. **Expected Output**: 
   - After completing the setup, the Jenkins Dashboard should appear.

### Step 3: Configure Jenkins Master-Slave Cluster
1. **Configure Jenkins Global Security**:
   - Go to **Manage Jenkins > Configure Global Security** and change **Agents** to **Random**. Click **Save**.

2. **Add Slave Node (Slave-1)**:
   - Go to **Manage Jenkins > Manage Nodes > New Node**.
   - Name the node "Slave-1" and select "Permanent Agent". Click **OK**.
   - Under **Launch Method**, select **Launch agent via Java Web Start**.

   Add the directory path for the working directory:
   ```bash
   /home/ubuntu/jenkins
   ```

3. **Expected Output**: You should now see Slave-1 in the Jenkins Dashboard as a connected node.

### Step 4: Add Slave-2 Node
1. **Add Slave-2 Node**:
   - Repeat the process for Slave-2. Copy the configuration from Slave-1 and click **OK**.
   
2. **Expected Output**: Slave-2 will now appear as a connected node on the Jenkins Dashboard.

### Step 5: Set Up File Transfer with FileZilla
1. **Install FileZilla**: 
   - Download and install FileZilla on your local machine.

2. **Set up connection for Slave-1**:
   - Open FileZilla and configure the connection with Slave-1's IP, username as "ubuntu", and port 22. 
   - Add the private key file (PPK file) under **Edit > Settings > SFTP**.
   
3. **Transfer Agent File**:
   - Go to the Jenkins Dashboard and click on Slave-1. Download the `agent.jar` file.
   - Drag and drop the `agent.jar` file from your local machine to the Slave-1 folder using FileZilla.

4. **Verify File Transfer**:
   - SSH into Slave-1 using PuTTY and check if the `agent.jar` file was transferred correctly:
     ```bash
     ls /home/ubuntu/jenkins/
     ```
     Expected output:
     ```
     agent.jar
     ```

### Step 6: Install OpenJDK on Slaves
1. **Install OpenJDK** on both Slave-1 and Slave-2:
   ```bash
   sudo apt-get install openjdk-11-jdk
   ```
   Expected output:
   ```
   Reading package lists... Done
   Building dependency tree       
   Reading state information... Done
   The following additional packages will be installed:
   ...
   ```

### Step 7: Connect Slave-1 and Slave-2 to Jenkins
1. **Connect Slave-1 to Jenkins**:
   - In the Jenkins Dashboard, click on Slave-1, copy the command line shown, and run it on the Slave-1 terminal:
     ```bash
     java -jar /home/ubuntu/jenkins/agent.jar -jnlpUrl http://<Master_IP>:8080/computer/Slave-1/slave-agent.jnlp
     ```
     Expected output:
     ```
     JNLP agent connecting to server at <Master_IP>:8080
     Connected
     ```

2. **Repeat the same for Slave-2**:
   - Run the command on Slave-2:
     ```bash
     java -jar /home/ubuntu/jenkins/agent.jar -jnlpUrl http://<Master_IP>:8080/computer/Slave-2/slave-agent.jnlp
     ```
     Expected output:
     ```
     JNLP agent connecting to server at <Master_IP>:8080
     Connected
     ```

### Step 8: Create CI/CD Pipeline Triggered by Git Webhooks
1. **Install Docker on Slaves**:
   - Install Docker on both Slave-1 and Slave-2:
     ```bash
     sudo apt-get install docker.io
     ```
     Expected output:
     ```
     Reading package lists... Done
     Building dependency tree       
     Reading state information... Done
     ```

2. **Create a New Job in Jenkins**:
   - Go to Jenkins Dashboard and click **New Item**. Name the project **Test**, select **Freestyle Project**, and click **OK**.
   
3. **Configure Git Repository**:
   - Under **Source Code Management**, select **Git** and enter your GitHub repository URL.
   - Restrict the project to Slave-1 for execution:
     ```bash
     Restrict where this project can be run: Slave-1
     ```

4. **Execute Shell**:
   - In the **Build** section, add the following command to deploy your project:
     ```bash
     docker run -d -p 82:80 --name devopsIQ apache2
     ```

5. **Expected Output**:
   - When the job runs, you will see a blue circle in the build history, indicating that the build was successful.

### Step 9: Verify Deployment on Slave-1
1. **Check Deployment**: Open a browser and go to `http://<Slave-1_IP>:82`.
   - You should see the Apache default page.

2. **Verify DevOpsIQ Deployment**: Go to `http://<Slave-1_IP>:82/devopsIQ` to see your deployed website.

### Step 10: Create Another Job for Slave-2
1. **Create Another Job** for Slave-2:
   - Create a new **Freestyle Project** for Slave-2 and configure it similarly to the Test job.

2. **Configure GitHub Webhook**:
   - In the "Source Code Management" section, add the same GitHub repository URL.
   - Restrict the execution to **Slave-2**.

3. **Execute Shell on Slave-2**:
   - Add the following execution shell:
     ```bash
     docker run -d -p 82:80 --name devopsIQ apache2
     ```

4. **Expected Output**: The deployment should now work on Slave-2 as well, and you should be able to access it at `http://<Slave-2_IP>:82/devopsIQ`.

### Step 11: Configure Job Triggering (Test -> Prod)
1. **Configure Test Job to Trigger Prod Job**:
   - In the "Test" job, go to **Configure** and add a post-build action to trigger the **Prod** job when the "Test" job is successful.

2. **Expected Output**: When the "Test" job is completed, the "Prod" job should be triggered automatically.

### Step 12: Set up GitHub Webhook for Triggering Jenkins Jobs
1. **GitHub Webhook Configuration**:
   - Go to your GitHub repository settings, then **Webhooks** > **Add webhook**. Use the Jenkins server address to trigger the build.

2. **Commit and Push** changes to the GitHub repository:
   - Modify the `index.html` file on your local machine, then run:
     ```bash
     git add .
     git commit -m "Modified index.html"
     git push
     ```

3. **Expected Output**: The Jenkins job should be triggered automatically after the push to GitHub.

### Final Step: Verify Changes
1. **Verify in Browser**:
   - Refresh your browser to see the updates (e.g., changed background image in the website).

---

Congratulations! You have successfully completed the Jenkins hands-on tutorial with commands and expected outputs.
