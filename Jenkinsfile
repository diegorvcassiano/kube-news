pipeline {
    agent any

    stages {
        stage ("Build docker image"){
            steps {
                script {
                    dockerapp = docker.build("diegorvcassiano/kube-news:v${env.BUILD_ID}", "-f ./src/Dockerfile ./src") 
                }
            }
        }
    }
}