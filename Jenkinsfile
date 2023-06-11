pipeline {
    agent any
    
    parameters {
        choice(
            choices: ['Dev', 'Prod'],
            description: 'Select the target cluster',
            name: 'TARGET_CLUSTER'
        )
    }
    
    stages {
        stage('Deploy to Kubernetes AKS') {
            steps {
                script {
                    // Print the contents of the workspace directory
                    sh 'ls -R'
                    
                    // Retrieve the selected target cluster
                    def kubeconfig
                    def clusterName = "${params.TARGET_CLUSTER}".toLowerCase()
                    
                    // Set the kubeconfig based on the selected target cluster
                    if (clusterName == 'dev') {
                        kubeconfig = credentials('kubeconfig-dev-k8s')
                    } else if (clusterName == 'prod') {
                        kubeconfig = credentials('kubeconfig-prod-k8s')
                    } else {
                        error "Invalid target cluster selected: ${params.TARGET_CLUSTER}"
                    }
                    
                    // Rest of your deployment steps
                    withCredentials([file(credentialsId: kubeconfig, variable: 'KUBECONFIG')]) {
                        sh "kubectl config view --kubeconfig=$KUBECONFIG" // View Kubernetes configuration
                        sh "kubectl get namespaces --kubeconfig=$KUBECONFIG" // Get Kubernetes namespaces
                        sh "kubectl apply -f ${clusterName}/deployment.yaml --kubeconfig=$KUBECONFIG" // Apply deployment configuration
                        sh "kubectl apply -f ${clusterName}/service.yaml --kubeconfig=$KUBECONFIG" // Apply service configuration
                    }
                }
            }
        }
    }
}
