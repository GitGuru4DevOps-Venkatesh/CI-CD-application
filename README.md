# simple-java-maven-app

This repository is for the
[Build a Java app with Maven](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `jenkins/scripts` subdirectory
contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline.

Sure, let's break down the process with an example. For simplicity, I'll assume you have a basic Java application and you're familiar with Docker and Jenkins.

1. **GitHub Setup:**
   - Create a new repository on GitHub. Let's call it `my-java-app`.
   - Push your Java application code along with the `pom.xml` file to this repository.

2. **Docker Setup:**
   - Install Docker on your machine. You can download it from the official Docker website.
   - Verify the installation by running `docker --version` in your terminal.

3. **Docker Containers:**
   - For Jenkins, you can use the official Jenkins Docker image. Pull it using the command `docker pull jenkins/jenkins`.
   - For SonarQube and Nexus, you can also use their official Docker images. Pull them using `docker pull sonarqube` and `docker pull sonatype/nexus3` respectively.

4. **Docker Compose (Optional):**
   - If you're using Docker Compose, create a `docker-compose.yml` file in your project directory.
   - Define services for Jenkins, SonarQube, and Nexus. Here's a basic example:

```yaml
version: '3'
services:
  jenkins:
    image: jenkins/jenkins
    ports:
      - 8080:8080
  sonarqube:
    image: sonarqube
    ports:
      - 9000:9000
  nexus:
    image: sonatype/nexus3
    ports:
      - 8081:8081
```

5. **CI/CD Pipeline:**
   - In your Jenkins dashboard, create a new pipeline job.
   - Write a Jenkinsfile to define your CI/CD workflow. Here's a basic example:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Nexus Deployment') {
            steps {
                sh 'mvn deploy'
            }
        }
    }
}
```

6. **Integration with Docker:**
   - In your Jenkinsfile, you can use the `docker` command to spin up Docker containers for SonarQube and Nexus as needed.
   - Make sure the Jenkins container has Docker installed. You can do this by creating a custom Docker image for Jenkins with Docker installed.

7. **Networking and Ports:**
   - In your Docker Compose file, map the necessary ports for Jenkins, SonarQube, and Nexus to your host machine.

8. **Environment Variables and Secrets:**
   - Use Docker secrets or environment variables to manage sensitive information such as credentials and API keys.

9. **Testing and Validation:**
   - Run `docker-compose up` in your terminal to start your services.
   - Make a change to your Java application and push it to GitHub. Verify that Jenkins triggers the pipeline and executes the steps correctly.

10. **Monitoring and Logging:**
    - You can use Docker's built-in logging capabilities to monitor your containers. Run `docker logs <container_id>` to view the logs for a specific container.

Remember, this is a basic example and your actual setup might require additional configurations and steps based on your specific needs. Always refer to the official documentation for each tool for more detailed and up-to-date information. Happy coding! 🚀
