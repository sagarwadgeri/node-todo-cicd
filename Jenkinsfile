pipeline {
    agent { label 'dev-agent' }
    stages {
        stage('code') {
            steps {
                git url: 'https://github.com/sagarwadgeri/node-todo-cicd', branch: 'master'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'docker build . -t sagarwadgeri/node-todo-app-cicd:latest'
            }
        }
        stage('Login and Push image') {
            steps {
                echo 'Logging into Docker Hub and pushing image'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u \${dockerHubUser} -p \${dockerHubPassword}"
                    sh "docker push sagarwadgeri/node-todo-app-cicd:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

