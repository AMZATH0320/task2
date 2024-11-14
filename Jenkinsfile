pipeline {
    agent any                  # im selecting slave node labeled as Docker_node

    environment {
         version = "${GIT_COMMIT_HASH}-${GIT_BRANCH_NAME}"
         docker_image ="amzath0304/task:${GIT_COMMIT_HASH}-${GIT_BRANCH_NAME}"     #amzath0304 is Dockerhub_username/task is image_name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/AMZATH0320/task.git'      # GitHub Public Repository
            }
        }

        stage('Login to Dockerhub') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'docker_credential', variable: 'Docker_username_password')]) {
                // some block
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerfilepath = '.'
                    sh "sudo docker build -t 'amzath0304/task:${version}' ."
                }  
            }
        }

        stage('Push docker image') {
            steps {
                script {
                    sh "sudo docker push 'amzath0304/backend:${version}'"
                }
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                script {
                    sh "sudo docker rmi -f ${env.docker_image}"
                }
            }
        }

    }
 }