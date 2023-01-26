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

        stage ("Push docker image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("v${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}