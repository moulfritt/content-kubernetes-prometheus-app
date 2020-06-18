pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "fbruslon/comicbox"
    }
    stages {
        stage('Checkout SCM') {
            steps{
                checkout scm
            }
        }
        stage ('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage ('Push Docker image') {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                }
            }
        }
        stage ('Deploy Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(
                            kubeconfigId: 'kubeconfig',
                            configs: 'deployment.yml',
                            enableConfigSubstitution: true
                    )
                }
            }
        }
    }
}
