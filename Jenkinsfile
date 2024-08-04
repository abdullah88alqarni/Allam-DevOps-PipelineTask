pipeline {
    agent any
    
    environment {
        AZURE_CREDENTIALS = credentials('azure-service-principal')
        ACR_NAME = 'devncai'
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = 'latest'
        ACR_LOGIN_SERVER = "${ACR_NAME}.azurecr.io"
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/yourrepository.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.ACR_LOGIN_SERVER}/${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }
        
        stage('Login to Azure') {
            steps {
                script {
                    sh "az login --service-principal -u ${env.AZURE_CREDENTIALS_USR} -p ${env.AZURE_CREDENTIALS_PSW} --tenant ${env.AZURE_CREDENTIALS_TEN}"
                }
            }
        }
        
        stage('Push to ACR') {
            steps {
                script {
                    sh "az acr login --name ${env.ACR_NAME}"
                    docker.image("${env.ACR_LOGIN_SERVER}/${env.IMAGE_NAME}:${env.IMAGE_TAG}").push()
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
