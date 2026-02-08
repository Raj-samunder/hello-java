pipeline {
    agent any

    environment {
        DOCKERHUB_CRED = credentials('docker-hub-creds')
        GIT_CRED = credentials('c71ae4c6-b684-43b7-9039-0f500f4c84aa')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Raj-samunder/hello-java.git', credentialsId: 'c71ae4c6-b684-43b7-9039-0f500f4c84aa'
            }
        }

        stage('Build Java') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t rajsamunder2004/hello-java:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push rajsamunder2004/hello-java:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'sudo k3s kubectl apply -f deployment.yaml'
            }
        }
    }
}

