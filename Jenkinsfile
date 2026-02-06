pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Raj-samunder/hello-java.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'javac HelloWorld.java'
            }
        }

        stage('Run') {
            steps {
                sh 'java HelloWorld'
            }
        }
    }
}
