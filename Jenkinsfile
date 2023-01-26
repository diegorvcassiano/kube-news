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

        stage ("Deploy kubernetes") {
            environment {
                tag_version = "v${env.BUILD_ID}"
            }
            steps {
                withKubeConfig (credentialsId: 'josnada-devops-kubeconfig') {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/digital-ocean-deployment.yaml'
                    sh 'kubectl apply -f ./k8s/digital-ocean-deployment.yaml'
                }                
            }
        }
    }
}