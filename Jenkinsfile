pipeline {
    agent any
    environment {  
        DOCKER_HUB_LOGIN = credentials('docker-hub-oscar')
    }
    stages {
        stage('Automation') {
            steps {
               sh 'git clone -b main https://github.com/oskofo2022/Automation.git'
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
               sh 'chmod +x Automation/docker_build.sh'
               sh './Automation/docker_build.sh'
            }
        }
        stage('Docker Push to Docker-hub') {
            steps {
                sh 'chmod +x Automation/docker_push.sh'
                sh './Automation/docker_push.sh'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ssh-ec2']){
                    sh 'chmod +x Automation/deploy_to_ec2_compose.sh'
                    sh './Automation/deploy_to_ec2_compose.sh'
                }
            }
        }
        stage('Notify Telegram') {
            steps {
                sh 'chmod +x Automation/telegram.sh'
                sh './Automation/telegram.sh'
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
