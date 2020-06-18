DOCKER_IMAGE_NAME = "fbruslon/comicbox"
node {
    stage'Checkout SCM'
        checkout scm
    stage('Build Docker Image')
        app = bocker.build(DOCKER_IMAGE_NAME)

    stage'Push Docker Image'
        docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
        app.push("${env.BUILD_NUMBER}")
        app.push("latest")

    stage'DeployToProduction'
        kubernetesDeploy(
                kubeconfigId: 'kubeconfig',
                configs: 'deployment.yml',
                enableConfigSubstitution: true
        )
}
