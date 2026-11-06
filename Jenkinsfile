pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Jenkins') {
            steps {
                sh 'echo Jenkins pipeline is running successfully'
            }
        }
    }
}