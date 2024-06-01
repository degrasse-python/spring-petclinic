pipeline {
    agent {
      docker {          
         image 'maven:3.5.0'         
     } 
    }
    environment {
        DOCKER_IMAGE = "spring-petclinic"
        DOCKER_REPO  = "originbotanica.jfrog.io/jfroginterview-docker/"
        registryCredential = 'originbotanica.jfrog.io/jfroginterview-docker/'
        BUILD_NUMBER = 'latest'
    }

    stages {
        stage('Compile') {
            steps {
                script {
                    // Assuming a Maven project
                    sh 'mvn compile'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests with Maven
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    // Build the Docker image
                    dockerImage = docker.build imagename
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                    sh 'docker tag ${DOCKER_IMAGE} ${DOCKER_IMAGE}:latest'
                }
            }
        }

        stage('Push to JFrog Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_JENKINS_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh 'docker push ${DOCKER_REPO}${DOCKER_IMAGE}'
                    }
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')

                  }
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker images to free space
                sh 'docker image prune -f'
            }
        }
    }
}
