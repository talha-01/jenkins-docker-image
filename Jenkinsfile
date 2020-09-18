pipeline {
	agent { label "master" }
	environment {
		APP_REPO_NAME = "talhas/phonebook"
	}
	stages {
		stage('Build Docker Image') {
			steps {
				sh 'cd /home/ec2-user/jenkins-docker-image'
				sh 'docker build -t $APP_REPO_NAME:latest .'
				sh 'docker images'
			}
		}
		stage('Push Image to Docker Hub') {
			steps {
				sh 'docker login'
				sh 'docker push $APP_REPO_NAME:latest'
			}
		}
	}
}

