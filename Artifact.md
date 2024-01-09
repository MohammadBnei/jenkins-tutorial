### Exercise: Use Groovy Script to Archive and Stash Artifacts

#### Objective:

Write a Groovy script within a Jenkins Declarative Pipeline to execute a shell command that creates a build artifact, archive this artifact, and then conditionally stash it if the branch is 'develop'.

#### Instructions:

1. **Create a Build Stage**:
   - In your `Jenkinsfile`, define a stage that is responsible for your application build process and ensure it creates an artifact representative of the build (e.g., a compressed file).

2. **Groovy Scripting for Artifact Handling**:
   - Use Groovy scripting within the `post` build section of your Jenkins pipeline to establish post-build artifact handling.

3. **Archive the Artifact**:
   - Utilize the appropriate step provided by Jenkins pipeline's syntax to archive the artifact generated from the build.

4. **Stash the Artifact Conditionally**:
   - Incorporate a `script` block where you will implement Groovy logic to ascertain the current branch being built.
   - Inside this logic, if the branch is 'develop', use the corresponding step to stash the created artifact for later use in other stages or jobs.

5. **Implement and Test**:
   - Incorporate your changes in the `Jenkinsfile`, commit it to the 'develop' branch, and observe the execution in Jenkins.
   - Ensure that the artifact archiving is working as intended and that the stashing occurs only when the 'develop' branch is built.

#### Deliverables:

- An updated `Jenkinsfile` that includes:
  - A defined build stage for the Node.js application.
  - A post-build action using Groovy script to archive the build artifact.
  - Conditional logic within the Groovy script to stash the artifact based on the branch condition.

Completing this exercise requires researching specific Jenkins pipeline steps for artifact handling and employing Groovy to implement conditional logic. This task will enhance understanding of the Jenkins pipeline's capabilities and Groovy scripting.