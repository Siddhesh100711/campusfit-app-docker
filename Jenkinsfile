pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out successfully'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image'

                sh '''
                docker build -t campusfit-app:${BUILD_NUMBER} .
                docker tag campusfit-app:${BUILD_NUMBER} campusfit-app:latest
                '''
            }
        }

    }
}
