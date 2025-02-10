pipeline {
    agent any

    stages {
      stage("Build Start") {
        steps {
          script {
            withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
              discordSend description: """
                Jenkins Build Start
                """,
                link: env.BUILD_URL, 
                title: "${env.JOB_NAME} : ${currentBuild.displayName} 시작", 
                webhookURL: "$discord_webhook"
            }
          }
        }
      }
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
            sh 'docker compose down'
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
				    withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
                        discordSend description: """
                        제목 : ${currentBuild.displayName}
                        결과 : ${currentBuild.currentResult}
                        실행 시간 : ${currentBuild.duration / 1000}s
                        """,
                        link: env.BUILD_URL, result: currentBuild.currentResult, 
                        title: "${env.JOB_NAME} : ${currentBuild.displayName} 성공", 
                        webhookURL: "$discord_webhook"
            }
		    }
		    
		    failure {
				    withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
                        discordSend description: """
                        제목 : ${currentBuild.displayName}
                        결과 : ${currentBuild.currentResult}
                        실행 시간 : ${currentBuild.duration / 1000}s
                        """,
                        link: env.BUILD_URL, result: currentBuild.currentResult, 
                        title: "${env.JOB_NAME} : ${currentBuild.displayName} 실패", 
                        webhookURL: "$discord_webhook"
            }
		    }
    }
}