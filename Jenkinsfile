pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/flask-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(env.DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    post {
        success {
            // Example of sending an email notification
            emailext (
                subject: "Deployment Successful",
                body: "The master branch has been successfully deployed via Jenkins.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
        failure {
            emailext (
                subject: "Deployment Failed",
                body: "There was an issue deploying the master branch. Please check the Jenkins logs.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}
