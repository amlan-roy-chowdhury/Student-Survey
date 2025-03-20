pipeline {
    agent any

    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"  // Ensure Jenkins can access kubeconfig
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/amlan-roy-chowdhury/Student-Survey.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("arc2233/studentsurvey:0.1")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-credentials') {
                        dockerImage.push('0.1')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/survey-3-loadbalancer.yaml'
            }
        }
    }

    post {
        success {
            echo "Deployment Successful! 🚀"
        }
        failure {
            echo "Deployment Failed! Check logs for details."
        }
    }
}
