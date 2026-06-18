pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning source code'

                git branch: 'main',
                    url: 'https://github.com/Siddhesh100711/campusfit-app-docker.git'
            }
        }

    }
}
