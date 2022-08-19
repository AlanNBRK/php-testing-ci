pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-hub-cred-alannbrk')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t alannbrk/php-image:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push alannbrk/php-image:latest'
			}
		}
		
		stage('Ansible Deploy') {

			steps {
				ansiblePlaybook disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.ini', playbook: 'deploy-docker.yml'
			}
		}
		
			
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
