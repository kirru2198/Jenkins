# 📘 Assignment 3: Jenkins Build Pipeline Overview

Tasks To Be Performed:
 1. Create a pipeline in Jenkins
 2. Once push is made to “develop” a branch in Git, trigger job “test”. This will copy Git files to test node
 3. If test job is successful, then prod job should be triggered
 4. Prod jobs should copy files to prod node

## ✅ Objective:
The goal of Assignment 3 is to **create and visualize a Jenkins pipeline** using the **Build Pipeline Plugin**. This will help you understand how different Jenkins jobs can be chained and visualized in a flow.

---

## 🛠️ Prerequisites:
- Jenkins is installed and running.
- At least two jobs already created (e.g., `Push to Test` and `Push to Prod` from Assignment 2).
- Nodes are properly configured and Java is installed on all.

---

## 🔌 Step 1: Install the Build Pipeline Plugin

1. Navigate to **Manage Jenkins**.
2. Click on **Manage Plugins**.
3. Go to the **Available** tab.
4. Search for **Build Pipeline Plugin**.
5. Select it and click **Install without restart**.

> 🔎 **Note:** If the plugin is already installed, you'll find it in the **Installed** tab.

---

## 🧱 Step 2: Create a Pipeline View

1. From the Jenkins Dashboard, click on **+ New View** (top-left).
2. Enter a name, e.g., `PipelineView`.
3. Select **Build Pipeline View** as the type.
4. Click **OK**.

---

## 🧰 Step 3: Configure the Pipeline View

After creating the view:

1. Set the **Displayed Name** of the pipeline.
2. Set the **First Job in Pipeline** (e.g., `Push to Test`).
3. Configure additional options like:
   - Number of builds to display.
   - Refresh frequency.
   - Show pipeline parameters (optional).

Click **Apply** and then **OK** to save the configuration.

---

## 🧪 Step 4: Test the Pipeline

1. Push code to the relevant GitHub branch (e.g., `develop` or `test`).
2. Trigger the first job in the pipeline.
3. Observe how the pipeline flows from one job to another:
   - `Push to Test` triggers → copies files to **test node**.
   - `Push to Prod` triggers → copies files to **prod node**.

---

## 🧵 How This Ties into Previous Assignments

- **Assignment 1**: Set up Jenkins Master and Slave nodes.
- **Assignment 2**: Created jobs to push to different branches and configured SCM triggers.
- **Assignment 3**: Visualizes the job flow using a Build Pipeline view.

---

## ❗ Important Notes

- Make sure the jobs are **correctly configured** with proper GitHub URLs and branch names.
- Without triggering the job at least once, you won’t see the workspace directories created.
- This assignment is mostly **visual** and **plugin-based**; the logic has already been implemented in earlier tasks.

---

## 📞 Need Help?

If you're unable to complete this assignment:

- Try replicating steps in a clean Jenkins instance.
- If issues persist, raise a **ticket for a 1-on-1 session** with your trainer.

---

## 🙏 Closing Remarks

> Great job completing Assignments 1 and 2! Assignment 3 is a visual wrap-up of your automation workflow. You're one step closer to mastering Jenkins pipelines. Keep going! 🚀

---
