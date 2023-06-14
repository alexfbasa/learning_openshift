pipeline {
  agent any

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/alexfbasa/learning_openshift.git'
      }
    }

    stage('Build Docker image') {
      steps {
        script {
          def dockerHubCredentialsId = 'DockerHub_Credentials'
          def dockerImage = docker.build("alexsimple/nginx")
          withDockerRegistry([credentialsId: dockerHubCredentialsId, url: '']) {
            docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-token') {
              dockerImage.push()
            }
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([string(credentialsId: 'OpenShift_Credentials', variable: 'TOKEN')]) {
          script {
            sh '''
              oc login https://192.168.99.100:8443 --token $TOKEN --insecure-skip-tls-verify
              oc project "My Project"
              oc apply -f openshift-template.yaml
            '''
          }
        }
      }
    }
  }
}
