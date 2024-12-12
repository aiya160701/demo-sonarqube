pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	environment {
		SONAR_PROJECT_KEY = 'demo'
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
	}

	stages {
		stage('Checkout Github'){
			steps {
				git branch: 'main', credentialsId: 'jenkins-private-key', url: 'https://github.com/aiya160701/demo-sonarqube.git'
			}
		}
		
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Tests'){
			steps {
				sh 'npm test'
			}
		}
		stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'final-demo-token', variable: 'SONAR_TOKEN')]) {
				   
					withSonarQubeEnv('SonarQube') {
						sh """
                  				${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                  				-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    				-Dsonar.sources=. \
                   				-Dsonar.host.url=http://184.73.92.123:9001 \
                    				-Dsonar.login=${SONAR_TOKEN}
                    				"""
					}	
				}
			}
		}
	}
	post {
		success {
			echo 'Build completed succesfully!'
		}
		failure {
			echo 'Build failed. Check logs.'
		}
	}
}
