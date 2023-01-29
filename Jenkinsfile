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
            environment{
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }  

        stage ('Deploy Kubernetes') {
            steps {
                withKubeConfig ([credentialsId: 'kubeconfig'])  {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s-manifests/deployment-api.yaml'
                    sh 'kubectl apply -f ./k8s-manifests/'
                }              
            }
        }

    }
    

}
