pipeline {
    agent any                  

    environment {
         DOCKER_REPO = 'amzath0304/task'    
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/AMZATH0320/task.git'      
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
                    // Extract commit hash and branch name
                    def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def branchName = env.BRANCH_NAME

                    // Build the Docker image and tag it with commit hash and branch name
                    def imageName = "${DOCKER_REPO}:${commitHash}"
                    def imageTag = "${DOCKER_REPO}:${branchName}"
                }  
            }
        }

        stage('Push docker image') {
            steps {
                script {
                    // Push the image to Docker Hub with both commit hash and branch tag
                    def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def branchName = env.BRANCH_NAME
                    def imageName = "${DOCKER_REPO}:${commitHash}"
                    def imageTag = "${DOCKER_REPO}:${branchName}"
                    
                    // Push the images
                    sh "docker push ${imageName}"
                    sh "docker push ${imageTag}"
                }
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                script {
                    // Clean up Docker images and unused resources after pushing
                    sh 'docker system prune -af'
                }
            }
        }

    }
 }
