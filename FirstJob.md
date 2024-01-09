# **IV. Jenkins Job Basics**

Creating a job, also known as a project, is the fundamental interaction you'll have with Jenkins. A job can be anything from building projects to executing shell commands or scripts. Let's discuss how to set up a simple Jenkins job.

## **IV.1 Creating a Basic Job**

1. **New Item:** To create a new job, click on "New Item" on the Jenkins dashboard. The name you provide will be used to identify the job within Jenkins. Select 'Freestyle project,' then click "OK" to confirm.

2. **Job Configuration:** Once you've created a job, a configuration page will appear. Here you can control the job's behavior. The "Description" field can be used to provide a brief summary of the job's tasks.

## **IV.2 Configuring Source Code Management (SCM)**

This depends largely on where you're storing your codebase.

1. **Git:** If you're using Git, you can input your repository URL, specify the branches to build, and provide access credentials if required.

## **IV.3 Configuring Build Triggers**

Under 'Build Triggers,' you decide when Jenkins will run your job. Job can run:

1. **Periodically:** Set a schedule using cron-like notation. For example, `H * * * *` would run the job every hour.

2. **After other projects are built:** If your job has a dependency on others, you can specify those projects in this section.

3. **GitHub hook trigger for Git SCM polling:** This is a build trigger for when changes are pushed to GitHub.

4. **On-demand:** Leaving all these boxes unchecked will configure the job to run only when manually triggered.

In the subsequent steps, you can customize further parameters, build steps, and post-build actions according to the needs of your project. The process we've outlined is to get you familiar with the basic job configuration and triggers in Jenkins. As you dig deeper into Jenkins, you'll encounter more complex configurations that allow for robust and intricate build pipelines.

# **VII. Introduction to Jenkinsfile**

A Jenkinsfile is text file that contains the definition of a Jenkins Pipeline. It's checked into source control, allowing you to treat Pipeline code like any other code and giving you the advantages of versioned code, review, edit, and iteration on your delivery pipeline. It uses the Pipeline Groovy DSL and can be written in either Declarative or Scripted syntax.

## **Exercise 1: Basic Syntax**

For our first exercise, we're going to become familiar with writing a Jenkinsfile in Declarative Syntax. Open your preferred text editor.

Write your first Jenkinsfile, just like the basic pipeline, but, this time, save the code as a file named "Jenkinsfile".

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the project...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
            }
        }
    }
}
```
Save this Jenkinsfile and commit it to the root of your source repository.

## **Exercise 2: Pipeline Options**

We can also specify options that apply to the entire pipeline. These change the default settings of the pipeline.

For example, let's add a retry option. If any stage in the pipeline fails, Jenkins will retry it up to two more times before giving up:

```groovy
pipeline {
    agent any

    options {
        retry(2)
    }

    stages {
        //... (the rest of the pipeline)
    }
}
```
Save this change, commit it to your repository, and push the changes.

## **Exercise 3: Post Section**

The `post` section is where we can put actions to execute after the completion of the pipeline or a stage.

Modify your Jenkinsfile to send an email if the pipeline fails:

```groovy
pipeline {
    agent any

    stages {
        //... (the rest of the pipeline)
    }

    post {
        failure {
            mail to: 'user@example.com',
                 subject: 'Pipeline Failure',
                 body: 'The pipeline has failed.'
        }
    }
}
```

Experiment with different post-condition blocks: `always`, `changed`, `fixed`, `regression`, `aborted`, `success`, `unstable`, `unsuccessful`, and `cleanup`.

# **Exercise 4: Environment Variables**

Environment variables are key components of any CI/CD pipeline. They help in configuring the build, defining important parameters, and storing sensitive data. 

Create a Jenkinsfile with an environment section that sets a variable:

```groovy
pipeline {
    agent any

    environment {
        MY_VARIABLE = 'Hello, Jenkins!'
    }

    stages {
        stage('Print') {
            steps {
                echo "${MY_VARIABLE}"
            }
        }
        //... (the rest of the pipeline)
    }
}
```

In this exercise, you define an environment variable `MY_VARIABLE` and then print it out in the `Print` step. 

## **Exercise 5: Groovy Script**

Integrate Groovy scripts in your pipeline which gives you a way to tackle complex scenarios. To run Groovy scripts in your Jenkinsfile, you have to wrap the Groovy script command within a `script` block.

Modify your Jenkinsfile to include a simple Groovy script as a part of the `Print` stage:

```groovy
pipeline {
    agent any

    stages {
        stage('Print') {
            steps {
                script {
                    def dateTime = new Date()
                    echo "Current date and time: ${dateTime}"
                }
            }
        }
        //... (the rest of the pipeline)
    }
}
```

In this example, Groovy's native ability to handle date and time operations is demonstrated. The Groovy script creates an instance of `Date()` (which will be set to the current date and time), and then it is printed out using an `echo` step.

# **Exercise 6: Extracting version from package.json**

Retrieving the version from the `package.json` file is a common requirement, especially when you want to tag your build or maintain versioned deployments. You will use Groovy's built-in JsonSlurper to parse JSON.

Modify your Jenkinsfile to extract and print the version of a Node.js project:

```groovy
pipeline {
    agent any

    stages {
        stage('Parse') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    def version = packageJson.version
                    echo "The version of this project is ${version}"
                }
            }
        }
        //... (the rest of the pipeline)
    }
}
```

In this script, `readJSON` is used to parse the `package.json` file and extract the `version` value. JsonSlurper returns this file's content as a data structure of lists and maps, enabling easy access via standard dot notation.

Ensure the `Pipeline Utility Steps` plugin, which provides the `readJSON` function, is installed in Jenkins.

Through this exercise, you should understand how to parse JSON files, specifically extracting data from `package.json`. This skill is essential for Jenkinsfile configurations in various DevOps cycles.
