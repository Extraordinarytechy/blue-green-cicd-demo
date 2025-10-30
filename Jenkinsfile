pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'extraordinarytechy/myapp:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',                      
                    credentialsId: 'github-token',        
                    url: 'https://github.com/Extraordinarytechy/blue-green-cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'      
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-creds', variable: 'DOCKER_TOKEN')]) {
                    sh '''
                    echo $DOCKER_TOKEN | docker login -u extraordinarytechy --password-stdin
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy Blue') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-minikube', variable: 'KUBECONFIG')]) {
                    dir('k8s') {
                        sh '''
                        kubectl apply -f blue-deployment.yaml --kubeconfig=$KUBECONFIG
                        kubectl apply -f service.yaml --kubeconfig=$KUBECONFIG
                        '''
                    }
                }
            }
        }

        stage('Deploy Green') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-minikube', variable: 'KUBECONFIG')]) {
                    dir('k8s') {
                        sh '''
                        kubectl apply -f green-deployment.yaml --kubeconfig=$KUBECONFIG
                        '''
                    }
                }
            }
        }

        stage('Switch Traffic to Green') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-minikube', variable: 'KUBECONFIG')]) {
                    dir('k8s') {
                        sh '''
                        kubectl apply -f switch-to-green.yaml --kubeconfig=$KUBECONFIG
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo ' Blue-Green Deployment successful! Traffic switched to Green environment.'
        }
        failure {
            echo ' Deployment failed. Check Jenkins logs for details.'
        }
    }
}
