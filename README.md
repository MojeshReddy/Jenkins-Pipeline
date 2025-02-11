# Complete CI/CD pipeline for a Java application, using Maven, SonarQube, Argo CD, Docker and Kubernetes

![image](https://github.com/RachanaVenkat/java-app-cicd/assets/151712438/df08e8f3-a2e1-4c0b-b99c-588801983055)

CI stands for Continuous Integration and CD stands for Continuous Delivery/Deployment. It is the process of automating the integration of code changes from different sources to a single codebase and testing if it is deployable, followed by deploying for the end-users.

## Here's the detailed explanation of the entire pipeline:
### Step 1 - 
   Checking out the code from SCM, it is this repo in my project

### Step 2 - 
  Either configure GitHub webhooks or directly provide this repo as the script path in Jenkins for triggering the Jenkins pipeline to begin the process to automatic building and testing.
  
### Step 3 - 
  Use Maven targets i.e., `maven clean package` to build the java application on the Docker image chosen as agent(complete pipeline is executed on this Docker Image). But don't have to install since Maven is pre-installed on the Docker Image.
  
### Step 4 - 
   Now, install SonarQube to perform static code ananlysis, code scanning and checking for vulnerabilities. If any of the tests failed, configure Jenkins plug-ins to send out alerts/notifications.
   
### Step 5 - 
   If the previous stage is passed, build the Docker Image of this applicationa and push the image to any docker regisrtries like DockerHub, ECR etc.
   
------------------This marks the end of Continuous Integration--------------------------

### Step 6 - 
   Now that we have the final image in the registry, we need to monitor it constantly for changes and this can be done using a shell-script or Argo Image Updator. I have used a shell-script here. Ansible, which is a configuration management tool can be used too, but it doesn't have the ability to constantly monitor.
   
### Step 7 - 
   Whenever there is a change in the image in the registry, the shell-script catches it and updates the manifests repository, which is `deployment.yml` in this project. This new commit made to this manifests repo, keeps the image updated.
   
### Step 8 - 
   Here comes the main character of our pipeline i.e., Argo CD. It is responsible for maintaining state between the manifests folder and the kubernetes cluster on which the application is deployed. Argo CD uses the manifests as the single source of truth and enables proper deployment of the application and maintentance of the kubernetes cluster-infrastructure.

### Step 9 -
   Finally, Argo CD deploys the application on to the kubernetes cluster and application will be running on 2 pods as specified in `deployment.yml`

   # **Complete CI/CD Pipeline for a Java Application**

## **Introduction**
This repository demonstrates a **CI/CD pipeline** for a Java application using **Maven, SonarQube, Argo CD, Docker, and Kubernetes**. The pipeline automates **code integration, testing, security scanning, containerization, and deployment** to a Kubernetes cluster.

---
## **Technologies Used**
- **Maven** → Build automation & dependency management
- **SonarQube** → Static code analysis & security checks
- **Docker** → Containerization & image registry
- **Kubernetes** → Orchestration & deployment
- **Argo CD** → GitOps-based deployment & state management
- **Jenkins** → CI/CD automation tool

---
## **Pipeline Overview**
### **Continuous Integration (CI)**
#### **Step 1: Checkout Code from SCM**
- The source code is fetched from the GitHub repository.

#### **Step 2: Trigger Jenkins Pipeline**
- Configure **GitHub Webhooks** or set the repo as the script path in Jenkins.
- This triggers the pipeline whenever a new commit is pushed.

#### **Step 3: Build the Java Application with Maven**
- The application is built inside a **Docker container** using Maven:
  ```sh
  mvn clean package
  ```
- No manual installation of Maven is required since it's pre-installed in the Docker image.

#### **Step 4: Static Code Analysis with SonarQube**
- SonarQube performs:
  - **Static code analysis**
  - **Code scanning**
  - **Security vulnerability checks**
- If issues are found, Jenkins **notifies developers** via configured plugins.

#### **Step 5: Build and Push Docker Image**
- If all tests pass, create a Docker image:
  ```sh
  docker build -t myapp:latest .
  ```
- Push the image to a **Docker registry** (DockerHub, AWS ECR, etc.):
  ```sh
  docker push myrepo/myapp:latest
  ```

🚀 **This marks the end of the Continuous Integration (CI) phase.**

---

### **Continuous Deployment (CD)**
#### **Step 6: Monitor Image Changes in the Registry**
- A **shell script** monitors for new image versions.
- Alternative: Use **Argo Image Updater** for automated monitoring.

#### **Step 7: Update Kubernetes Manifests**
- The shell script updates **`deployment.yml`** to reference the new image.
- This ensures the Kubernetes deployment always runs the latest image.

#### **Step 8: Argo CD Syncs with Kubernetes Cluster**
- **Argo CD** continuously monitors the **manifests repository** and syncs it with the Kubernetes cluster.
- `deployment.yml` acts as the **single source of truth** for infrastructure and application configuration.

#### **Step 9: Deploy the Application to Kubernetes**
- Argo CD deploys the application, running it on **two pods** as specified in `deployment.yml`.
- The application is now live and accessible.

---

## **How to Run the Pipeline**
1. **Clone the repository:**
   ```sh
   git clone https://github.com/your-repo/ci-cd-java.git
   ```
2. **Setup Jenkins and configure webhooks** for auto-triggering builds.
3. **Ensure SonarQube, Docker, Kubernetes, and Argo CD are properly installed and running.**
4. **Run the pipeline manually or commit new changes** to trigger Jenkins.

---

## **Conclusion**
This **CI/CD pipeline** automates the entire process from **code integration to deployment**, ensuring:
✅ Faster development cycles 🚀
✅ Improved code quality 🔍
✅ Seamless deployment & scaling 🏗️

Feel free to contribute or report any issues! Happy coding! 😊


