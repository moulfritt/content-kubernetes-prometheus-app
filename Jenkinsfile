pipeline {
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
                app = bocker.build(DOCKER_IMAGE_NAME)
            }
        }
        stage ('Push Docker image') {
            steps {
                docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
        }
        stage ('Deploy Kubernetes') {
            steps {
                kubernetesDeploy(
                        kubeconfigId: 'kubeconfig',
                        configs: 'deployment.yml',
                        enableConfigSubstitution: true
                )
            }
        }
    }
}
