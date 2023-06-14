pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t your-image-name .'
      }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'your-registry-credentials-id', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
          sh "docker login -u $USERNAME -p $PASSWORD your-registry-url"
          sh 'docker push your-image-name'
        }
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'your-openshift-credentials-id', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
          sh 'oc login your-openshift-endpoint -u $USERNAME -p $PASSWORD'
          sh 'oc project your-project'
          sh 'oc apply -f your-deployment-config.yaml'
        }
      }
    }
  }
}