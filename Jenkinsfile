pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/shubham-automation/deployments.git', branch:'main'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("chaudharishubham2911/nginx:${env.BUILD_NUMBER}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          env.IMAGE_TAG = ${env.BUILD_NUMBER} 
          kubernetesDeploy(configs: "k8s-deployment.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
