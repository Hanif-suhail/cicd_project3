pipeline {
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Using the ID set in Jenkins
        DOCKER_IMAGE = 'hanifsuhail/task_management_app' // Replace with your DockerHub username
    }
    stages {
        stage("checkout to feature branch") {
            steps {
                script {
                     checkout scmGit(branches: [[name: '*/feature']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/Hanif-suhail/cicd_project3']])
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    // Log in to DockerHub
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    
                    // Push Docker image to DockerHub
                    sh 'docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}'
                }
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                // Clean up the Docker images to save space
                sh 'docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo "Docker image ${DOCKER_IMAGE}:${BUILD_NUMBER} has been successfully pushed to DockerHub."
        }
        failure {
            echo "Build failed!"
        }
    }
}


