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
	
    stage('Deploying to K3S') {
	  environment {
          registryCredential = 'dockerhub-credentials'
      }
      steps {
        script {
          kubernetesDeploy(kubeconfigId: 'k3s-token', configs: "php-sample.yaml",
		  dockerCredentials: [
                        [credentialsId: registryCredential]
                 ]		  
		  )
        }
      }
    }
  }
}