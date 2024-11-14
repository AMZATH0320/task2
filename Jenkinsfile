pipeline {
    agent any      

    parameters {
        // Define a parameter for the branch nameeeee
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch name to build')            

    environment {
         DOCKER_REPO = 'amzath0304/task'    
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                     // Debugging: Print branch name and current directory
                    echo "Branch Name: ${params.BRANCH_NAME}"
                    echo "Current Workspace: ${pwd()}"
                    // Clonei the GitHub repository
                    git branch: "${env.BRANCH_NAME}", url: 'https://github.com/AMZATH0320/task.git'
                }
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
                    // Debugging: Ensure we are in the correct directory
                    echo "Building Docker image..."
                    echo "Current Workspace: ${pwd()}"

                    // Check if Docker is available on the Jenkins agent
                    sh 'docker --version'

                    // Extract commit hash and branch name
                    def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def branchName = params.BRANCH_NAME ?: 'unknown'  // Use 'unknown' if BRANCH_NAME is null

                    // Debugging: Print commit hash and branch name
                    echo "Commit Hash: ${commitHash}"
                    echo "Branch Name: ${branchName}"

                    // Build the Docker image and tag it with commit hash and branch name
                    def imageName = "${DOCKER_REPO}:${commitHash}"
                    def imageTag = "${DOCKER_REPO}:${branchName}"

                    // Build the Docker image
                    sh "docker build -t ${imageName} -t ${imageTag} ."
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
