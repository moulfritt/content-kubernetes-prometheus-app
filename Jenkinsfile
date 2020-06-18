DOCKER_IMAGE_NAME = "fbruslon/comicbox"
node {
    parameters {
        booleanParam(name: 'Delete', defaultValue: false, description: 'Delete ???')
    }
    stage('Checkout SCM') {
        checkout scm
    }
    stage('Build Docker Image') {
        steps {
            scripts {
                app = bocker.build(DOCKER_IMAGE_NAME)
            }
        }
//        if (expression {params.Action} == "Delete") {
//                delete_resource = true
//                echo "delete resource value: ${delete_resource}"
//        }
    }
    stage('Push Docker Image') {
        steps {
            scripts {
                docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
        }
    }
    tage('DeployToProduction') {
        steps {
            kubernetesDeploy(
                kubeconfigId: 'kubeconfig',
                configs: 'deployment.yml',
                enableConfigSubstitution: true
            )
        }
    }
}