### Setting Up Java for Jenkins: Step-by-Step Notes

<img width="531" alt="image" src="https://github.com/user-attachments/assets/ca10ea6f-9b25-48f4-a9b5-48ae4d22b53a" />


1. **Using Installed Java**:
   - Since Java is already installed on your system, you will specify which version to use (e.g., Java 17 or Java 11).

2. **Finding Java Installation Path**:
   - The Java installation is typically found in a directory like `/usr/lib/jvm/java-17-openjdk-amd64/`.
   - You can check the contents of this directory to confirm that Java is installed correctly.

3. **Setting the JAVA_HOME Variable**:
   - You need to set an environment variable called `JAVA_HOME` to point to the Java installation path.
   - This variable tells other programs (like Jenkins) where to find Java.

4. **Exporting JAVA_HOME**:
   - You will use the command `export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64/` to set the variable.
   - This command makes the `JAVA_HOME` variable available to the current session.

5. **Verifying JAVA_HOME**:
   - To check if the variable is set correctly, you can run `echo $JAVA_HOME` in the terminal. It should display the path you just set.

6. **Setting the PATH Variable**:
   - The `PATH` variable is a list of directories that the system checks when you run a command.
   - You need to add the Java `bin` directory to the `PATH` so that Jenkins can find the Java executables.
   - You can do this by running:
     ```bash
     export PATH=$PATH:$JAVA_HOME/bin
     ```

7. **Verifying the PATH Variable**:
   - To confirm that the Java path has been added, run `echo $PATH`. You should see the Java `bin` directory included in the output.

8. **Configuring Jenkins**:
   - Open Jenkins and go to the configuration settings.
   - You can name your Java installation (e.g., "My Java" or "Test Java").
   - Set the `JAVA_HOME` in Jenkins to the path you defined earlier (`/usr/lib/jvm/java-17-openjdk-amd64/`).

9. **Why Set JAVA_HOME and PATH?**:
   - Setting `JAVA_HOME` and updating the `PATH` ensures that Jenkins can find and use the Java installation without any issues.
   - If these variables are not set correctly, you might encounter errors like "command not found" when Jenkins tries to run Java commands.

10. **Final Steps**:
    - After configuring Jenkins with the Java path, you can proceed with your project setup.
    - Make sure to test the configuration by running a simple Java command in Jenkins to ensure everything is working correctly.

### Summary
By following these steps, you have successfully set up Java for Jenkins, ensuring that Jenkins can locate and use the Java installation without any issues. This setup is crucial for running Java-based applications and builds within Jenkins.
