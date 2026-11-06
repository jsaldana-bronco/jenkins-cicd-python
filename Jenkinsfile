pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Python') {
            steps {
                sh 'python3 --version'
            }
        }

        stage('Run Python Unit Tests') {
            steps {
                sh 'python3 -m unittest discover -v'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESSFUL - All Python tests passed!'
        }
        failure {
            echo 'BUILD FAILED - Check the console output.'
        }
    }
}