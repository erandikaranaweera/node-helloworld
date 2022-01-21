pipeline{
  environment {
    registry = ("erandiranaweera/nodehelloworld").toLowerCase()
    registrycredential = 'docker-hub'
    dockerimage = ''
  }
  agent any
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
                  dockerimage = docker.build("erandiranaweera/nodeapp").toLowerCase()
                }
             }
          }
          stage('Push Image') {
              steps{
                  script 
                    {
                        docker.withRegistry( '', registrycredential ) {
                            dockerimage.push()
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
