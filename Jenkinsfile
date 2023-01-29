pipeline {
    agent any

    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("afmaia/kube-news:${env.BUILD_ID}", '-f ./src/dockerfile ./src')
                }
            }
        }  

        stage ('Push Docker Image') {

            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                    dockerapp.Push('latest')
                    dockerapp.Push("${env.BUILD_ID}")
                }
            }
        }  

    }
    

}
