Creating and adding a Docker agent in Jenkins allows you to run your Jenkins pipeline steps inside a Docker container. This can provide a clean, controlled environment for your builds or tests each time your job runs. Here's a step-by-step tutorial on how to achieve this:

### Step 1: Ensure Docker is Installed
Firstly, confirm that Docker is installed on the Jenkins host. You can verify this by running `docker --version` from the command line. If Docker is not installed, follow the [official Docker documentation](https://docs.docker.com/engine/install/) to install it.

### Step 2: Update Jenkins Configuration
Ensure that Jenkins itself is configured to allow the use of Docker. If you're running Jenkins as a Docker container, it needs to be run with the correct permissions to access the Docker daemon from inside the container.

### Step 3: Install Jenkins Docker Plugin
1. In Jenkins, navigate to 'Manage Jenkins' > 'Manage Plugins'.
2. Go to 'Available' tab and search for 'Docker plugin'.
3. Select the plugin and click 'Install without restart'.

### Step 4: Configure Docker Cloud in Jenkins
1. Go to 'Manage Jenkins' > 'Manage Nodes and Clouds'.
2. Click on ‘Configure Clouds’.
3. Add a new cloud by clicking on 'Add a new cloud' and select 'Docker'.
4. Configure the Docker Cloud details:
    - **Name**: Enter a name for your Docker cloud.
    - **Docker Host URI**: Specify the URI for your Docker host. For example, use `unix:///var/run/docker.sock` for the local Unix socket or `tcp://<host>:<port>` for a TCP socket.

### Step 5: Configure Docker Agent Template
Within the Docker Cloud configuration, you'll need to add a Docker Agent template, which specifies the Docker image and settings for the agents:

1. Click on 'Docker Agent templates...' and then 'Add Docker Template'.
2. Fill in the details:
    - **Labels**: Unique identifier for the Jenkins agent (used in your Jenkins pipeline to select the correct agent).
    - **Name**: A name for the template.
    - **Docker Image**: The image to use for the agent (e.g. `node:14-alpine` if you want an agent with Node.js 14 available).
    - **Remote Filing System Root**: Path in the container where Jenkins establishes the workspace (e.g., `/home/jenkins/agent`).
3. Under 'Launch method', select 'Docker Once Retention Strategy' to remove the container after use or choose another strategy as needed.
4. Configure any other settings according to your requirements (e.g., volume bindings, environment variables, etc.).
5. Save the configuration.

### Step 6: Use the Docker Agent in a Jenkins Pipeline
Now that you have a Docker agent configured, you can use it in your Jenkins pipeline by specifying the label in a `pipeline` block's `agent` section:

```groovy
pipeline {
    agent {
        docker {
            label 'my-docker-agent'
            image 'node:14-alpine'
            args '-v /home/jenkins/agent:/home/jenkins/agent'
        }
    }
    stages {
        // define your pipeline stages here
    }
}
```

### Step 7: Test Your Pipeline
Create a new pipeline job or use an existing one, add the Jenkinsfile from Step 6, and run the job. Jenkins will execute the job using the configured Docker agent.

### Step 8: Troubleshoot if Necessary
If the job doesn't run as expected, check the following:
- Verify the Docker agent configuration in Jenkins.
- Confirm that the Docker image specified is accessible and can be pulled by Jenkins.
- Review the build log for any errors that occurred while trying to run the job on the Docker agent.

By following these steps, you should successfully set up a Docker agent in Jenkins that can be used to isolate your build and test environment. This setup ensures greater consistency across builds and can be tailored to various build environments by using different Docker images.