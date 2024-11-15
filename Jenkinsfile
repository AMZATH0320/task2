pipeline {
    
    agent any      
    
    environment {
        DOCKER_REPO = 'amzath0304/task' 
       

    }

    stages {
        stage('Clone Repository') {
            steps {
                script { 
                    echo "Current Workspace: ${pwd()}"
                    // Clone the GitHub repository to user AMZATH0320 Public repo
                    git branch: 'main', url: 'https://github.com/AMZATH0320/task2.git'
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
                    commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    imagetag = "${commitHash}-${branchName}"
                    docker_image = "${DOCKER_REPO}:${imagetag}"
                    echo "Generated Docker image: ${docker_image}"

                    // Build the Docker image with two tags (commit hash and branch name)

                    sh "sudo docker build -t '${docker_image}' ."       
                }  
            }
        }

        stage('Push docker image') {
            steps {
                script {
                    // pushing the Docker image
                    echo "Pushing Docker image: ${docker_image}"

                    sh "sudo docker push '${docker_image}'"
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
