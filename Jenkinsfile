pipeline {
    agent {
        docker {
            image 'jenkins/jenkins:lts'
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount Docker socket for Docker commands
        }
    }

    environment {
        //KUBECONFIG = credentials('kubeconfig-credentials-id') // This should be the ID of your Jenkins credential for Kubeconfig
        KUBECONFIG = credentials('Mykubeconfig_for_k8s_On_DockerDesktop') // This should be the ID of your Jenkins credential for Kubeconfig  
    }

    stages {
        stage('Install kubectl') {
            steps {
                script {
                    // Download and install kubectl
                    sh '''
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x kubectl
                        mv kubectl /usr/local/bin/
                    '''
                    // Verify kubectl installation
                    sh 'kubectl version --client'
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Assuming your Kubernetes YAML files are in the k8s directory
                    def k8sDir = "k8s_frontend_backend_app"
                    
                    sh "kubectl apply -f ${k8sDir}/"
                    //sh "kubectl apply -f ${k8sDir}/service.yaml"
                    //sh "kubectl apply -f ${k8sDir}/deployment.yaml"
                    // Add additional kubectl apply commands as needed
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}