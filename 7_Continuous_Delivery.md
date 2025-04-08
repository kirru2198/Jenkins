# Continuous Integration and Continuous Delivery (CI/CD) with Jenkins

## Recap of What We've Covered

### 1. **Continuous Integration (CI)**
   - **CI** is a development practice that requires developers to merge code into the main branch regularly.
   - By integrating frequently, problems are caught early, making it easier to maintain code quality.

### 2. **Continuous Delivery (CD)**
   - **CD** involves the automated delivery of code into production. 
   - **Continuous Delivery** requires manual approval for deployment, while **Continuous Deployment** automatically deploys the code into production without human intervention.

### 3. **Jenkins Setup**
   - We installed Jenkins on a server and resolved a **Java version compatibility issue** (Java 11 did not work, but Java 17 did).
   - Jenkins was configured to create jobs for compiling, testing, and packaging.

### 4. **Jenkins Pipeline**
   - We created a **Jenkins Pipeline** where:
     - **GitHub Webhook** triggers the build process (compile → test → package).
   - We learned about **upstream and downstream jobs**, where jobs are triggered automatically based on success or failure.

### 5. **Jenkins Master-Slave Architecture**
   - The **Master-Slave** architecture helps distribute workloads. 
   - The **Master** controls the Jenkins instance, and the **Slave** executes the actual jobs.
   - We configured **passwordless SSH** between the master and slave servers for smooth communication.

### 6. **Passwordless SSH Setup**
   - We generated **SSH keys** on the master server and copied the public key to the slave server.
   - This setup enables Jenkins to communicate with the slave server without needing a password each time.

### 7. **Adding Slave Servers to Jenkins**
   - We added slave servers to Jenkins via the **Manage Jenkins → Manage Nodes → New Node** interface.
   - We used **labels** to direct jobs to specific slave servers.

### 8. **Running Jobs on Slaves**
   - Jobs were successfully run on slave servers, ensuring efficient resource utilization and distribution of work.

---

## What's Next?

Now, we will set up a **Continuous Integration and Continuous Delivery (CI/CD)** pipeline using a real-world project. We will manually configure each step, test it, and automate the process with Jenkins.

### Topics We’ll Cover Soon:
   - **Best Practices for Git Branching**:
     - Managing test, development, and production environments using Git branches.
   - **Automatic Deployments**:
     - Setting up automatic deployments after successful tests.
   - **Maven**:
     - How to use **Maven** for packaging and deploying Java projects (which Jenkins will automate for us).

If there are specific parts of Git or Jenkins that you’d like to dive deeper into, feel free to ask!

---

### Important Note:
- **Maven Snapshots**:
  - A **Snapshot** version in Maven is a work-in-progress version of your project that is not yet finalized.
  - You can specify it in your `pom.xml` file to use a snapshot version of your project.

---

## Today's Focus: **Setting Up Jenkins with Apache Tomcat**

We will now focus on setting up a **Jenkins pipeline** that packages and deploys a Java application to **Apache Tomcat**.

### Breakdown of Tasks:
1. **Job 1**: Package the Java application into a deployable artifact (e.g., a `.war` or `.jar` file).
2. **Job 2**: Deploy the packaged artifact to an Apache Tomcat server.

### What is Apache Tomcat?
- **Apache Tomcat** is an open-source web server used to run Java-based web applications. It is similar to Nginx or Apache HTTP server, but specifically designed for Java applications.

---

### Step-by-Step: Installing Apache Tomcat on a Slave Server

We'll install **Tomcat** on the slave server to serve our Java application. The installation process is as follows:

1. **Download Apache Tomcat**:
   - Use **wget** to download the Apache Tomcat `.tar.gz` file.
   - We will download it to the `/opt` directory on the slave server.
   
   ```bash
   wget https://downloads.apache.org/tomcat/tomcat-9/v9.x.x/bin/apache-tomcat-9.x.x.tar.gz
   ```

2. **Extract the File**:
   - Use the **tar** command to extract the `.tar.gz` file.
   
   ```bash
   tar -xzvf apache-tomcat-9.x.x.tar.gz
   ```

3. **Remove the Compressed File**:
   - Clean up by removing the `.tar.gz` file.
   
   ```bash
   sudo rm -rf apache-tomcat-9.x.x.tar.gz
   ```

4. **Navigate to Tomcat Directory**:
   - Change to the extracted directory.
   
   ```bash
   cd /opt/apache-tomcat-9.x.x
   ```

5. **Start Tomcat**:
   - Go to the **bin** directory and run the `startup.sh` script to start Tomcat.
   
   ```bash
   cd bin
   chmod 755 startup.sh
   ./startup.sh
   ```

6. **Stop Tomcat**:
   - To stop Tomcat, use the `shutdown.sh` script.
   
   ```bash
   chmod 755 shutdown.sh
   ./shutdown.sh
   ```

---

### Accessing Tomcat

- Tomcat’s default port is **8080**, so you can access it by navigating to `http://<IP>:8080` in your web browser.
- If you're using a **cloud instance** (like EC2), ensure that the security group allows inbound traffic on port 8080.

---

### Handling Port Conflicts (Jenkins and Tomcat)

Since both **Jenkins** and **Tomcat** use port 8080 by default, we encountered a port conflict. To resolve this:
1. **Change Jenkins Port**:
   - Modify Jenkins' port by editing `/etc/default/jenkins` and changing the port number from `8080` to `9090`.
   - Restart Jenkins:
   
   ```bash
   systemctl stop jenkins
   systemctl start jenkins
   ```

2. **Check and Fix Port**:
   - To check if port 8080 is free, use the following commands:
   
   ```bash
   netstat -tuln
   lsof -i :8080
   ```

---

### Starting Tomcat After Fixing Ports

- Once Jenkins is stopped and port 8080 is free, you can start Tomcat:
  
```bash
cd /opt/apache-tomcat-9.x.x/bin
./startup.sh
```

- **Default Tomcat Page**: When you access `http://<IP>:8080`, you will see the default Tomcat page, which confirms that Tomcat is working properly.

---

### Configuring Tomcat for Remote Access

By default, Tomcat only allows access from the **local machine**. To allow remote access:
1. **Modify `context.xml`**:
   - Go to Tomcat’s installation directory and modify the `context.xml` file to allow access from other machines.
   
2. **Edit `tomcat-users.xml`**:
   - To manage user access, edit `tomcat-users.xml` to add roles and credentials.
   
   Example:
   ```xml
   <role rolename="admin"/>
   <user username="admin" password="admin" roles="admin"/>
   ```

---

## Next Steps

1. **Deploying the Application**:
   - We’ll create Jenkins jobs for packaging and deploying the Java application on Tomcat.
   
2. **Managing Jenkins Plugins**:
   - We'll discuss managing Jenkins plugins, including updates to the **Pipeline** and **Declarative Pipeline** plugins.

3. **Next Session**:
   - Tomorrow, we will finish setting up **Continuous Deployment** and dive deeper into Jenkins administration and GitLab.

---

### Final Thoughts

We’ve made significant progress by setting up Jenkins and Apache Tomcat. You now know how to install and configure Tomcat, manage port conflicts, and enable remote access. In the next session, we’ll continue by deploying our Java application using Jenkins.

