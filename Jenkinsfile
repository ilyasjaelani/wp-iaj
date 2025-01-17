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
        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ilyas-k3s', contextName: '', credentialsId: 'ilyas-k3s', namespace: 'default', serverUrl: 'https://172.16.25.129:6443']]) {
          sh '''
            curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"
            chmod u+x ./kubectl
            ./kubectl apply -k ./ -n $KUBERNETES_NAMESPACE
            sleep 60
          '''
        }
      }
    }
    stage('View Namespaces') {
      steps {
        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ilyas-k3s', contextName: '', credentialsId: 'ilyas-k3s', namespace: 'default', serverUrl: 'https://172.16.25.129:6443']]) {
          sh '''
            ./kubectl get all -n $KUBERNETES_NAMESPACE
          '''
        }
      }
    }
  }
}
