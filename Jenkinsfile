pipeline {
    agent any
    environment {
        // Define your Docker Hub credentials and image name here
        KUBE_CONTEXT = 'ilyas-wordpress'  // Kube context if you have multiple clusters
        KUBERNETES_NAMESPACE = 'ilyas-wordpress'  // Replace with your namespace
    }
    stages {
        //stage('Checkout') {
        //    steps {
        //        // Checkout your repository
        //        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'git@github.com:ilyasjaelani/wp-iaj.git']])
        //    }
        //}
        stage('View Namespaces') {
            steps {
                script {
                    // Create namespace on Kubernetes using kubectl
                    sh '''
                        kubectl get all -n  $KUBERNETES_NAMESPACE
                    '''
                }
            }
        }
        stage('View Pods') {
            steps {
                script {
                    // Create namespace on Kubernetes using kubectl
                    sh '''
                        kubectl describe pods ilyas-wordpress-mysql-fb58f47d-xflzh -n $KUBERNETES_NAMESPACE
                    '''
                }
            }
        }
    }
}

