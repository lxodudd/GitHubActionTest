pipeline {
    agent any

    stages {
      stage("Copy Environment Variable File") {
        steps {
          script {
            withCredentials([file(credentialsId: 'env-file', variable: 'env_file')]) {
              sh 'cp $env_file .env'
              sh 'chmod 644 .env'
            }           
          }
        }
      }

      stage("Docker Image Build & Container Run") {
        steps {
          script {
            sh 'docker compose build'
            sh 'docker compose up -d'
          }
        }
      }
    }
    
    post {
		    always {
				    echo '항상 실행된다.'
		    }
		    
		    success {
				    echo '성공 시 실행된다.'
		    }
		    
		    failure {
				    echo '실패 시 실행된다.'  
		    }
    }
}