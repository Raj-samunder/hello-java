pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('TestDocker') // DockerHub creds in Jenkins
    }

    stages {
        stage("Git Checkout") {
            steps {
                git 'https://github.com/yourusername/hello-world-java.git'
            }
        }

        stage("Maven Build") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "docker build -t YOUR_DOCKERHUB_USERNAME/hello-world-java:latest ."
                sh "docker push YOUR_DOCKERHUB_USERNAME/hello-world-java:latest"
            }
        }

        stage("K8s Deployment") {
            steps {
                sh "kubectl apply -f k8s-deploy.yml"
            }
        }
    }

    post {
        always {
            node {
                script {
                    if(currentBuild.currentResult == 'FAILURE') {
                        step([$class: 'Mailer', 
                              notifyEveryUnstableBuild: true, 
                              recipients: 'Test@test.com', 
                              sendToIndividuals: true])
                    }
                }
            }
        }
    }
}
