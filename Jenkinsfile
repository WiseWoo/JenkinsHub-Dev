pipeline {
    agent any
		
		tools {
				maven 'my_maven' 
		}

		environment {
				GITNAME='WiseWoo'
				GITMAIL='jiwooo0@naver.com'

				// HTTP 접속을 위한 정보 - Clone을 받기 위한 용도
				GITWEB='https://github.com/WiseWoo/JenkinsHub-Dev.git'

				// SSH 접속을 위한 정보 - Push를 하기 위한 용도
				GITSSHADD='git@github.com:WiseWoo/JenkinsHub-Dev.git'
				GITCREDENTIAL='git_cre'
		}
		
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}