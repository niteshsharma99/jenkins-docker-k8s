pipeline {
    environment {
        KUBECONFIG = credentials('kubeconfig-prod-k8s')
        githubCredential = 'GitHub-Creds'
    }  
    agent any
    
    stages {
        stage('Deploy to Kubernetes AKS') {
            steps {
                script {
                    // Print the contents of the workspace directory
                    sh 'ls -R'
                    
                    // Rest of your deployment steps
                    withCredentials([file(credentialsId: 'kubeconfig-prod-k8s', variable: 'KUBECONFIG')]) {
                        sh "kubectl config view --kubeconfig=$KUBECONFIG" // View Kubernetes configuration
                        sh "kubectl get namespaces --kubeconfig=$KUBECONFIG" // Get Kubernetes namespaces
                        sh "kubectl apply -f deployment.yaml --kubeconfig=$KUBECONFIG" // Apply deployment configuration
                        sh "kubectl apply -f service.yaml --kubeconfig=$KUBECONFIG" // Apply service configuration
                    }
                }
            }
        }
    }
}
