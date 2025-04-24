# Jenkins Installation Guide

This guide walks you through the steps to set up Jenkins on an AWS EC2 instance. You can follow these instructions for both Linux and Windows environments. For this guide, we focus on the Linux setup, but feel free to refer to the documentation for other platforms.

### Minimum System Requirements
- **RAM**: 2 GB (For testing purposes, 1 GB may suffice, but 2 GB is recommended for smooth performance.)
- **Disk Space**: 4 GB free (For testing, 10 GB is recommended, but Jenkins will work with 4 GB of disk space).
- **Java**: Jenkins requires Java to be installed (JDK 8 or higher).

### Recommended System Specifications
- **RAM**: 4 GB
- **Disk Space**: 50 GB
- **Java**: JDK 8 or higher.

---

### Installation Steps

1. **Log into your AWS Console**:
    - Launch an EC2 instance with your desired configuration.
    - Use a T2 micro instance for testing (2 GB RAM, 1 vCPU).
    - Configure security groups to allow inbound traffic on port 8080 (Jenkins default port).

2. **Check Java Installation**:
    - Jenkins is written in Java, so you must install Java first.
    - Run the following command to verify if Java is already installed:
      ```bash
      java -version
      ```
    - If Java is not installed, you can install it using the following command:
      ```bash
      sudo apt-get install openjdk-17-jdk
      ```

3. **Update System Repositories**:
    - Before installing Jenkins, update your system repositories to ensure you have the latest package information:
      ```bash
      sudo apt-get update
      ```

4. **Add the Jenkins Repository**:
    - Download and install the Jenkins repository key:
      ```bash
      sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
      ```
    - Add the Jenkins repository to your system:
      ```bash
      echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
      ```

5. **Install Jenkins**:
    - Run the following command to install Jenkins:
      ```bash
      sudo apt-get update
      sudo apt-get install jenkins
      ```

6. **Start Jenkins Service**:
    - After installation, start the Jenkins service using the following command:
      ```bash
      sudo systemctl start jenkins
      ```

7. **Enable Jenkins to Start on Boot**:
    - To ensure Jenkins starts automatically after reboot, run:
      ```bash
      sudo systemctl enable jenkins
      ```

---

### Accessing Jenkins

1. **Open Port 8080 in Security Group**:
    - Go to the AWS Console, navigate to your EC2 instance's security group, and open TCP port 8080 to allow inbound traffic.

2. **Access Jenkins Web Interface**:
    - Once Jenkins is running, open a browser and go to:
      ```
      http://<your-ec2-ip>:8080
      ```
    - You should see the Jenkins homepage.

3. **Unlock Jenkins with Initial Admin Password**:
    - Jenkins requires an initial admin password for setup. You can find it in the following file:
      ```
      /var/lib/jenkins/secrets/initialAdminPassword
      ```
    - Use the `cat` command to view the password:
      ```bash
      sudo cat /var/lib/jenkins/secrets/initialAdminPassword
      ```

4. **Complete the Setup Wizard**:
    - Copy the password and paste it into the setup wizard in the web interface.
    - Install Suggested Plugins: After unlocking Jenkins, you will be prompted to install suggested plugins. This process may take 5-7 minutes. The suggested plugins include essential tools that help Jenkins function optimally.
    - Create an Admin User: After the plugins are installed, you’ll be asked to create an admin user. This user will have full access to Jenkins. You can enter any username and password, e.g.,:
  
Username: admin

Password: your-password

Email: admin@example.com

Complete Setup: After creating the admin user, Jenkins will be ready to use, and you'll be directed to the Jenkins dashboard.

---

### Managing Jenkins

- **Start Jenkins**:
  ```bash
  sudo systemctl start jenkins
  ```

- **Stop Jenkins**:
  ```bash
  sudo systemctl stop jenkins
  ```

- **Restart Jenkins**:
  ```bash
  sudo systemctl restart jenkins
  ```

- **Check Jenkins Status**:
  ```bash
  sudo systemctl status jenkins
  ```

---

### Additional Notes

- **Security Group**: Ensure that port 8080 is open in your EC2 security group to access Jenkins via the web.
- **Amazon Linux**: If using Amazon Linux, ensure the correct repository URL for Jenkins is used.
- **Jenkins on EC2**: The lab setup on EC2 has a 3-hour time limit. Once the instance is stopped, any configurations will be lost. For a permanent setup, consider using your own AWS account or a local machine.

---

### Plugins and Further Configuration

Jenkins supports a wide range of plugins that enhance its functionality. During the setup process, you’ll be prompted to install suggested plugins. These include essential tools that you’ll need for CI/CD pipelines. We’ll go over managing and installing plugins in detail in a separate lab.

---

### Creating an AMI (Optional)

If you want to create an Amazon Machine Image (AMI) to preserve your configuration:

1. Go to your EC2 instance.
2. Select "Actions" -> "Image and Templates" -> "Create Image".
3. Use this image to launch new instances with your Jenkins setup.

---

### Conclusion

Once Jenkins is installed and running, you're ready to create and manage Jenkins pipelines. Keep in mind that if you stop and restart your EC2 instance, the IP address may change, so update your browser URL accordingly.

Now that Jenkins is set up and running, you can begin automating the CI/CD pipeline with Jenkins. In future sessions, we'll dive deeper into configuring Jenkins for specific use cases, integrating it with version control systems like Git, and setting up pipelines for your projects.


