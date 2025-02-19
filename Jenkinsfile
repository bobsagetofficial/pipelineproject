pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "bobsagetofficial/my-app"
  }

  stages {
    stage('Clone Respository') {
      steps {
        checkout([
          $class: 'GitSCM',
            branches: [[name: 'master']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'CloneOption', shallow: true]],
            submoduleCfg: [],
            userRemoteConfigs: [[
              credentialsId: GIT_DEPLOY_KEY,
              url: 'git@github.com:bobsagetofficial/pipelineproject.git'
            ]]
        ])
      }
    }

    stage('Build Docker Image') {
          steps {
            sh 'docker build -t $DOCKER_IMAGE:latest .'
          }
    }

    stage('Push Docker Image') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
          sh 'docker push $DOCKER_IMAGE:latest'
        }
      }
    }

    stage('Deoply to Kubernetes') {
      steps {
        sh 'helm upgrade --install my-app ./helm-chart'
      }
    }
  }
}
