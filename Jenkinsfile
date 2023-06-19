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

    stage('Create ConfigMap') {
      steps {
        script {
          sh 'oc get configmap nginx-config -o yaml > nginx-config.yaml'
          sh 'echo "---" >> nginx-config.yaml'
          sh 'oc create configmap nginx-config-update --from-file=nginx-config --dry-run=client -o yaml >> nginx-config.yaml'
          sh 'oc apply -f nginx-config.yaml'
          sh 'oc create configmap nginx-html --from-file=nginx-html'
          sh 'echo "---" >> nginx-html.yaml'
          sh 'oc create configmap nginx-html-update --from-file=nginx-html --dry-run=client -o yaml >> nginx-html.yaml'
          sh 'oc apply -f nginx-html.yaml'
        }
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
