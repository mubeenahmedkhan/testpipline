pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.image('nginx:latest').pull()
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('nginx:latest').run('-p 8080:80', 'nginx')
                }
            }
        }

        stage('Push Docker Image to GitHub') {
            steps {
                script {
                    docker.image('nginx:latest').push("${env.GITHUB_USER}/${env.JOB_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
