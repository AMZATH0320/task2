pipeline {
    
    agent any      
    
    environment {
        DOCKER_REPO = 'amzath0304/task' 
        commitHash = "sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()"
        branchName = "sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()"
        docker_image = "${DOCKER_REPO}:${commitHash}-${branchName}"

    }

    stages {
        stage('Clone Repository') {
            steps {
                script { 
                    echo "Current Workspace: ${pwd()}"
                    // Clone the GitHub repository to user AMZATH0320 Public repo
                    git branch: 'master', url: 'https://github.com/AMZATH0320/task.git'
                }
            }    
        }

        stage('Login to Dockerhub') {
            steps {
                 withCredentials([usernameColonPassword(credentialsId: 'docker_credential', variable: 'Docker_username_password')]) {
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {   
                    // Build the Docker image with two tags (commit hash and branch name)
                    sh "echo ${docker_image}"
                    sh "sudo docker build -t ${docker_image} ."       
                }  
            }
        }

        stage('Push docker image') {
            steps {
                script {
                    // push the Docker image
                    sh "sudo docker push ${docker_image}"
                }
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                script {
                    // Clean up Docker images and unused resources after pushing
                    sh 'sudo docker system prune -af'
                }
            }
        }
    }
 }
