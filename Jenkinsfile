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
  }
}

