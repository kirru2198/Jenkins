**Jenkins Installation Setup Notes**

---

### Minimum System Requirements

- **RAM**: 2 GB (1 GB may suffice for testing, but 2 GB is recommended for smooth performance)
- **Disk Space**: 4 GB free (10 GB recommended for testing)
- **Java**: JDK 8 or higher required

---

### Recommended System Specifications

- **RAM**: 4 GB
- **Disk Space**: 50 GB
- **Java**: JDK 8 or higher

---

### Installation Steps

1. **Log into AWS Console**:
   - Launch an EC2 instance (T2 micro recommended for testing).
   - Configure security groups to allow inbound traffic on port 8080.

2. **Check Java Installation**:
   - Verify Java installation:
     ```bash
     java -version
     ```
   - If not installed, install Java:
     ```bash
     sudo apt install openjdk-21-jre-headless
     ```

3. **Update System Repositories**:
   - Update package information:
     ```bash
     sudo apt-get update
     ```

4. **Add the Jenkins Repository**:
   - Download and install the Jenkins repository key:
     ```bash
     sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
     ```
   - Add the Jenkins repository:
     ```bash
     echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
     ```

5. **Install Jenkins**:
   - Install Jenkins:
     ```bash
     sudo apt-get update
     sudo apt-get install jenkins
     ```

6. **Start Jenkins Service**:
   - Start the Jenkins service:
     ```bash
     sudo systemctl start jenkins
     ```

7. **Enable Jenkins to Start on Boot**:
   - Enable Jenkins to start automatically:
     ```bash
     sudo systemctl enable jenkins
     ```

---

### Accessing Jenkins

1. **Open Port 8080 in Security Group**:
   - Open TCP port 8080 in your EC2 instance's security group.

2. **Access Jenkins Web Interface**:
   - Open a browser and go to:
     ```
     http://<your-ec2-ip>:8080
     ```

3. **Unlock Jenkins with Initial Admin Password**:
   - Find the initial admin password:
     ```
     /var/lib/jenkins/secrets/initialAdminPassword
     ```
   - View the password:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

4. **Complete the Setup Wizard**:
   - Paste the password into the setup wizard.
   - Install suggested plugins and create an admin user.

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

- Ensure port 8080 is open in your EC2 security group.
- If using Amazon Linux, verify the correct repository URL for Jenkins.
- Be aware of the 3-hour time limit for EC2 lab setups; configurations may be lost if the instance is stopped.

---

### Plugins and Further Configuration

- Jenkins supports various plugins for enhanced functionality.
- Suggested plugins will be installed during the setup process.

---

### Creating an AMI (Optional)

1. Go to your EC2 instance.
2. Select "Actions" -> "Image and Templates" -> "Create Image".
3. Use this image to launch new instances with your Jenkins setup.

---

### Conclusion

- After installation, you can start creating and managing Jenkins pipelines.
- Remember that stopping and restarting your EC2 instance may change the IP address.
