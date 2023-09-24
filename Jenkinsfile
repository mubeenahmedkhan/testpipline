pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.image('nginx:latest').pull()
                    docker.image('nginx:latest').tag("my-nginx:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("my-nginx:${env.BUILD_NUMBER}").run('-p 8080:80', 'nginx')
                }
            }
        }

        stage('Push Docker Image to GitHub') {
            steps {
                script {
                    def registryUrl = 'https://github.com/mubeenahmedkhan/testpipline.git' // GitHub Container Registry URL
                    def imageTag = "my-nginx:${env.BUILD_NUMBER}"
                    def credentialsId = 'mubeenahmedkhan'

                    docker.withRegistry(registryUrl, credentialsId) {
                        docker.image(imageTag).push()
                    }
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
}
