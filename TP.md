# Practical Introduction to Jenkins: Building and Deploying a Node.js Application with pnpm

### Objectives:
- Learn to install and configure Jenkins using Docker for a Node.js application.
- Understand how to set up a multibranch pipeline for continuous integration with Jenkins.
- Deploy the application via a pipeline to a hosting platform like Netlify or Vercel.
- Share Jenkins configuration by creating a Docker image and using Docker Compose.

### Requirements:
- Docker and Docker Compose installed on your local machine or server.
- Familiarity with Jenkins, Docker, Git, Node.js, and deployment processes.
- Access to a Git repository hosting a Node.js application.
- A Netlify or Vercel account for deployment.
- A Docker Hub account for pushing the custom Jenkins image.

### Instructions:

#### **Installation and Setup**
**1. Install Jenkins via Docker**
   - Obtain the Jenkins Docker image and instantiate a container with volume mounts for persistent storage.

**2. Complete Initial Jenkins Configuration**
   - Proceed through the Jenkins setup using the provided initial admin password.

#### **Plugin Installation and Tool Configuration**
**3. Install Required Plugins and Configure Node.js**
   - Add the NodeJS Plugin through 'Manage Plugins' and define a Node.js installation in 'Global Tool Configuration', including `pnpm` as a global package.

#### **Multibranch Pipeline Setup**
**4. Set Up Multibranch Pipeline Job**
   - Initiate a multibranch pipeline job in Jenkins and configure the source code management, pointing to your repository.

**5. Configure Branch Source and Job Triggers**
   - Specify branch discovery mechanisms and job triggers within the job’s configuration.

#### **Jenkinsfile and Deployment Setup**
**6. Prepare a Jenkinsfile**
   - Write a Jenkinsfile with pipeline stages including checkout, build, test, and deployment, committing it to your repository.

**7. Integrate Deployment with Hosting Platform**
   - Ensure the 'deploy' stage in Jenkinsfile contains deployment commands to Netlify or Vercel, secured with Jenkins credentials.

#### **Custom Jenkins Image Creation**
**8. Customize and Save Jenkins Configuration**
   - Once Jenkins is fully configured and your multibranch pipeline is operational, commit the configuration to a new Docker image.

**9. Push Jenkins Image to Docker Hub**
   - Tag and push the customized Jenkins Docker image to your Docker Hub repository.

#### **Docker Compose for Easy Setup**
**10. Create Docker Compose File**
   - Draft a `docker-compose.yml` file that specifies the Jenkins service, using the custom image from Docker Hub.

#### **Execution and Validation**
**11. Execute the Multibranch Pipeline**
   - Trigger the Jenkins pipeline either manually or via a commit to the repository.

**12. Confirm the Successful Deployment**
   - Visit the application URL provided by the hosting platform to ensure it is live and functioning.

#### **Deliverables:**
- A fully functional Dockerized Jenkins setup, tailored for Node.js builds with pnpm.
- A configured Jenkins multibranch pipeline job for continuous integration and deployment within the Jenkins UI.
- A `Jenkinsfile` in the repository with a pipeline that builds, tests, and deploys a Node.js application.
- A publicly accessible Node.js application deployed on Netlify or Vercel.
- A custom Docker image of your Jenkins setup pushed to Docker Hub.
- A `docker-compose.yml` file that allows for easy instantiation of your Jenkins environment.

### Docker Compose Example (`docker-compose.yml`):
```yaml
version: '3.8'
services:
  jenkins:
    image: your-dockerhub-username/jenkins-custom:latest
    ports:
      - "8080:8080"
    volumes:
      - /var/jenkins_home
```