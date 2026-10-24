pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Application') {
            steps {
                echo 'Checking application files'
                bat 'if exist index.html (echo index.html found) else (exit 1)'
            }
        }

        stage('Build Success') {
            steps {
                echo 'Continuous Integration Successful!'
            }
        }
    }
}
