pipeline {
  agent any

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/alexfbasa/learning_openshift.git'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t nginx_test .'
      }
    }

    stage('Push') {
      steps {
        withCredentials([string(credentialsId: 'jenkins-deployer', variable: 'TOKEN')]) {
          // Use the TOKEN variable for authentication with OpenShift
          sh "oc login https://192.168.99.101:8443 --token ${TOKEN} --insecure-skip-tls-verify"
          sh "oc project cp-0001"
          sh "oc apply -f openshift-template.yaml"
          sh "oc apply -f nginx-deployment.yaml"

          // Wait for the deployment to complete
          sh "oc rollout status dc/nginx-deployment --watch=true"
        }
      }
    }
  }
}
