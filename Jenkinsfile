pipeline {
    agent any
    environment {  
        DOCKER_HUB_LOGIN = credentials('docker-hub-roxs')
    }
    stages {
        stage('Automation') {
            steps {
               sh 'git clone -b master https://github.com/roxsross/automation.git'
            }
        }
        stage('install dependencies') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                echo "Init"
                sh 'npm install'
            }
        }
        stage('Unit Test') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh 'npm run test'
                }
            }
        } 
        stage('Docker Build') {
            steps {
               sh 'ls /automation/docker_build.sh'
               sh './automation/docker_build.sh'
            }
        }
        stage('Docker Push to Docker-hub') {
            steps {
                sh './automation/docker_push.sh'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ssh-ec2']){
                    sh './automation/deploy_to_ec2_compose.sh'
                }
            }
        }
        stage('Notify Telegram') {
            steps {
                sh 'chmod +x automation/telegram.sh'
                sh './automation/telegram.sh'
            }
        }

    } //--end stages

    // CLEAN WORKSPACES
    post { 
        always { 
            cleanWs()
        }
    }
}
