pipeline {
	agent { label "master" }
	environment {
		APP_REPO_NAME = "talhas/phonebook"
	}
	stages {
		stage('Pull the application') {
			steps {
				sh 'wget https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/Dockerfile'
				sh 'wget https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/app.py'
				sh 'wget https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/requirements.txt'
				sh 'wget -P templates https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/templates/add-update.html'
				sh 'wget -P templates https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/templates/delete.html'
				sh 'wget -P templates https://raw.githubusercontent.com/talha-01/docker-projects/master/001-docker-phonebook-application/webserver/templates/index.html'
			}
		}
		stage('Build Docker Image') {
			steps {
				sh 'docker build -t phonebook:latest .'
				sh 'docker tag phonebook:latest $APP_REPO_NAME:latest'
				sh 'docker tag phonebook:latest $APP_REPO_NAME:${BUILD_ID}'
				sh 'docker images'
			}
		}
		stage('Push Image to Docker Hub') {
			steps {
				withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
				sh 'docker push $APP_REPO_NAME:latest'
				sh 'docker push $APP_REPO_NAME:${BUILD_ID}'
				}
			}
		}
		stage('Renew deployment') {
			steps {
				script {
					sshagent(credentials : ['ssh']) {
						sh "echo pwd"
						sh 'ssh -t -t ubuntu@54.210.214.185 -o StrictHostKeyChecking=no "echo $TEST"'
					}
				}
			}
		}
	}
}

