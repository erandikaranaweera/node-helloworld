pipeline{
  environment {
    registry = ("erandiranaweera/nodehelloworld").toLowerCase()
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent windows
    stages {
        stage('Build'){
            steps{
                script{
                    sh 'npm install'
                }
            }
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build("erandiranaweera/nodeapp").toLowerCase()
                }
             }
          }
          stage('Push Image') {
              steps{
                  script 
                    {
                        docker.withRegistry( '', registryCredential ) {
                            dockerImage.push()
                        }
                   } 
               }
            }
        stage('Deploying into k8s'){
            steps{
                sh 'kubectl apply -f deployment.yml'
            }
        }
    }
}
