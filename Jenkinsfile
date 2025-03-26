pipeline {
  agent any
  environment {
    KUBE_CONTEXT = 'ilyas-wordpress'  // Kube context if you have multiple clusters
    KUBERNETES_NAMESPACE = 'ilyas-wordpress'  // Replace with your namespace
  }
  stages {
    stage('Checkout') {
      steps {
        // Checkout your repository
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'ilyas-github', url: 'git@github.com:ilyasjaelani/wp-iaj.git']])
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ilyas-k8s', contextName: '', credentialsId: 'ilyas-k8s-config', namespace: '$KUBERNETES_NAMESPACE', serverUrl: '']]) {
          sh '''
            kubectl apply -k ./ -n $KUBERNETES_NAMESPACE
            sleep 60
          '''
        }
      }
    }
    stage('View Namespaces') {
      steps {
        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ilyas-k8s', contextName: '', credentialsId: 'ilyas-k8s-config', namespace: 'KUBERNETES_NAMESPACE', serverUrl: '']]) {
          sh '''
            kubectl get all -n $KUBERNETES_NAMESPACE
          '''
        }
      }
    }
  }
}
