# Spring PetClinic Sample Application [![Build Status](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml/badge.svg)](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml)

## Understanding the Spring Petclinic application with a few diagrams

[See the presentation here](https://speakerdeck.com/michaelisvy/spring-petclinic-sample-application)

## Explaination of the Jenkinsfile 

1. *agent any*: This directive tells Jenkins to run the pipeline on any available agent.

2. *environment*: Defines environment variables, in this case, the Docker image name.

3. *stages*: Defines the different stages of the pipeline.

   - **Compile**: Runs the `mvn compile` command to compile the code.
   - **Test**: Runs the `mvn test` command to execute the tests.
   - **Package**: Builds the Docker image using the `docker build` command.
   - **Push to Registry**: Pushes the Docker image to my JFrog Docker registry. It uses credentials stored in Jenkins (specified by `docker-credentials-id`) to log in to the Docker registry.

4. *post*: Contains steps that run after the main pipeline stages. In this example, it cleans up any unused Docker images to free up disk space.

NOTES

- **JCenter Repository Configuration**: The JCenter repository is defined in the `pom.xml` under the `<repositories>` section. Maven will use this repository to resolve dependencies.a
- **Environment Variables**: The `DOCKER_IMAGE` environment variable specifies the Docker image name and tag.
- **Stages**: The pipeline includes stages for compiling the code, running tests, packaging the Docker image, and pushing the image to a Docker registry.
- **Credentials**: Configured Docker credentials in Jenkins and referenced them in the Jenkinsfile using the `credentialsId` to push to JFrog Artifactory.

## Explaination of the Dockerfile 

1. *Base Image*: The Dockerfile starts by specifying a base image. In this case, it uses `openjdk:11-jre-slim`, which is a slim version of the OpenJDK runtime.

2. *Working Directory*: It sets `/app` as the working directory inside the container using the `WORKDIR` instruction.

3. *Copy JAR File*: The `COPY` instruction copies the compiled JAR file from the host machine to the container. Adjust the path `target/your-artifact-id-1.0-SNAPSHOT.jar` to match the actual path and name of your JAR file.

4. *Expose Port*: The `EXPOSE` instruction specifies the port on which the application will run inside the container. Adjust the port number to match your application's configuration.

5. *Entry Point*: The `ENTRYPOINT` instruction specifies the command to run the JAR file when the container starts.

## Run Petclinic locally

Spring Petclinic is a sample spring boot based application that I have forked and created a docker image from. To test this image locally you should do the following.

1. Make sure you have Docker installed and running.

You can then access the Petclinic at <http://localhost:8080/>.

## Database configuration

In its default configuration, Petclinic uses an in-memory database (H2) which
gets populated at startup with data. The h2 console is exposed at `http://localhost:8080/h2-console`,
and it is possible to inspect the content of the database using the `jdbc:h2:mem:<uuid>` URL. The UUID is printed at startup to the console.

For this excerise we will just provide the proof of value docker image and not run any production database.

## Working with Petclinic Docker Image

Spring Petclinic is a sample spring boot based application that I have forked and created a docker image from. To test this image locally you should do the following.

### Prerequisites

The following items should be installed in your system:

- Docker Installed and Running


### Steps

1. Make sure you have Docker installed and running.

`docker image pull originbotanica.jfrog.io/jfroginterview-docker/spring-petclinic:latest`

`docker run -p 8080:8080 spring-petclinic:latest`
 
2. Navigate to the Petclinic (http ONLY)

    Visit [http://localhost:8080](http://localhost:8080) in your browser.

## License

The Spring PetClinic sample application is released under version 2.0 of the [Apache License](https://www.apache.org/licenses/LICENSE-2.0).
