pipeline {
    agent any

    environment {
        LOCAL_IMAGE = "kiii-jenkins-pipeline:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                sh 'docker build -t "$LOCAL_IMAGE" .'
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_TOKEN')]) {
                    sh 'docker tag "$LOCAL_IMAGE" "$DOCKERHUB_USERNAME/kiii-jenkins-pipeline:$BUILD_NUMBER"'
                    sh 'echo "$DOCKERHUB_TOKEN" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin'
                    sh 'docker push "$DOCKERHUB_USERNAME/kiii-jenkins-pipeline:$BUILD_NUMBER"'
                }
            }
        }
    }
}
