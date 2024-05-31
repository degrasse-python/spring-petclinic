pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "originbotanica.jfrog.io/jfroginterview-docker/spring-petclinic:latest"
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
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Push to JFrog Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_JENKINS_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh 'docker push ${DOCKER_IMAGE}'
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
