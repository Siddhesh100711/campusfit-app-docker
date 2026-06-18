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
        
	stage('Push') {
	    steps {
       		 echo 'Pushing image to Docker Hub'

        	 withCredentials([usernamePassword(
            	     credentialsId: 'dockerHubCred',
            	     usernameVariable: 'DOCKER_USER',
                     passwordVariable: 'DOCKER_PASS'
        	 )]) {

            	     sh '''
                	 echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

	                 docker tag campusfit-app:${BUILD_NUMBER} siddhesh0710/campusfit-app:${BUILD_NUMBER}

         	         docker tag campusfit-app:${BUILD_NUMBER} siddhesh0710/campusfit-app:latest
	 
         	         docker push siddhesh0710/campusfit-app:${BUILD_NUMBER}

                	 docker push siddhesh0710/campusfit-app:latest

                	 docker logout
            		'''
       		    }
   	    }
        }        

    }
}
