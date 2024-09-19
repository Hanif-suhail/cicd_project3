pipeline {
  agent {
    kubernetes {
      yamlFile 'jenkins-pod-template.yml' 
    }
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Using the ID set in Jenkins
    DOCKER_IMAGE = 'hanifsuhail/task_management_app' // Replace with your DockerHub username
    KUBERNETES_DEPLOYMENT_FILE = 'deployment.yml'
    KUBERNETES_SERVICE_FILE = 'service.yml'

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
        container('docker') {
                    // Build Docker image
          sh 'docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .'
        }
                
      }
    }

    stage('Push Docker Image to DockerHub') {
      steps {
        container('docker') {
                    // Log in to DockerHub
          sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    
                    // Push Docker image to DockerHub
          sh 'docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}'
        }
      }
    }

    stage('Cleanup Docker Images') {
      steps {
        container('docker') {
                // Clean up the Docker images to save space
          sh 'docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER}'
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        container('kubectl') {
                    // Use kubectl to apply the deployment and service YAML
          sh "kubectl apply -f ${KUBERNETES_DEPLOYMENT_FILE}"
          sh "kubectl apply -f ${KUBERNETES_SERVICE_FILE}"
        }
      }
    }
  }
}

