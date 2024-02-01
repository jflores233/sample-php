pipeline {
  environment {
    dockerimagename = "lightbulb233/php7.4.33-apache"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/jflores233/sample-php.git'
      }
    }
	

	stage('SonarQube Analysis') { 
		steps {
			script {
				withSonarQubeEnv('Sonarqube') {
					sh "${tool('sonar-scanner')}/bin/sonar-scanner -Dsonar.projectKey=myProjectKey -Dsonar.projectName=myProjectName"
				}
			}
		}
	}
			
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
      }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
  }
}