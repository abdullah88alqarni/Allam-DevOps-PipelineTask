pipeline {
    agent any
    
    environment {
        // Environment variables
        AZURE_CREDENTIALS_ID = 'Azure-Service-Principal'
        ACR_NAME = 'devncai'
        ACR_LOGIN_SERVER = 'devncai.azurecr.io'
    }
    
    stages {
        stage('Clone repository') {
            steps {
                // Clone the GitHub repository
                git url: 'https://github.com/abdullah88alqarni/Allam-DevOps-PipelineTask.git', branch: 'main' , credentialsId: 'github-creds'
            }
        }
        
        stage('Build Docker Images') {
            parallel {
                stage('Build Image 1') {
                    steps {
                        script {
                            // Build the first Docker image located in frontend folder
                           // docker.build("aahalqarni-frontend:latest", "./frontend")
                            sh 'echo test'
                        }
                    }
                }
                stage('Build Image 2') {
                    steps {
               //         script {
                            // Build the second Docker image located in backend folder
                           // docker.build("aahalqarni-backend:latest", "./backend")
                 //       }
                        sh 'echo test'
                    }
                }
            }
        }
        
        stage('Login to Azure Container Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'azure-login', usernameVariable: 'azure_username', passwordVariable: 'azure_pass')]) {
                        sh 'az login --service-principal -u $azure_username -p $azure_pass --tenant 58c64807-3315-4150-9fa2-84efc0ba24b6'              
                        sh "az acr login --name ${ACR_NAME}"
                    }
                }
            }
        }
        
        stage('Push Docker Images to ACR') {
            parallel {
                stage('Push Image 1') {
                    steps {
                        script {
                            // Tag and push the first Docker image
                            sh """
                                docker push aahalqarni-frontend:latest
                            """
                        }
                    }
                }
                stage('Push Image 2') {
                    steps {
                        script {
                            // Tag and push the second Docker image
                            sh """
                                docker push aahalqarni-backend:latest
                            """
                        }
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Cleanup Docker images from local system
            sh 'docker system prune -af'
        }
    }
}
