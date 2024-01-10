pipeline {
    agent any

    tools {
        maven 'my_maven' 
    }

    environment {
        GITNAME = 'WiseWoo'
        GITMAIL = 'jiwooo0@naver.com'
        GITWEBADD = 'https://github.com/WiseWoo/JenkinsHub-Dev.git'

        // GITSSHADD = 'git@github.com:WiseWoo/JenkinsHub-Dev.git'
				// Git Push는 SSH를 써야 가능하다. 그래서 SSH를 Push할 
        GITSSHADD = 'git@github.com:WiseWoo/JenkinsHub-Dep.git'
        GITCREDENTIAL = 'git_cre'
        //docker hub에 eks 레포지토리 꼭 깔아놔야함
        DOCKERHUB = 'jiwoo0/eks'
        DOCKERHUBCREDENTIAL = 'docker_cre'
    }

    stages {
        stage('Checkout Github') {
            steps {
			
				slackSend (channel: '#jenkins-notice', color: '#FFFF00', message:
                "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

			
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [],
                userRemoteConfigs: [[credentialsId: GITCREDENTIAL, url: GITWEBADD]]])
            }
            post {
                failure {
                    echo 'Repository clone failure'
                }
                success {
                    echo 'Repository clone success'
                }
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
        stage('image build'){
            steps{
                // sh "docker build -t WiseWoo/JenkinsHub-Dev:1.0 ."
                sh "docker build -t ${DOCKERHUB}:${currentBuild.number} ."
                sh "docker build -t ${DOCKERHUB}:latest ."
            }
        }
        stage('image push'){
            steps{
                withDockerRegistry(credentialsId: DOCKERHUBCREDENTIAL, url: '') {
                    sh "docker push ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker push ${DOCKERHUB}:latest"
                }
            }
            
            post {
                failure {
                    echo 'docker image push fail'
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
                
                success {
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
            }
        }
    }
}