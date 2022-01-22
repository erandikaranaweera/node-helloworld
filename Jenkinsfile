pipeline {
    agent any
    environment {
        PROJECT_ID = 'cloudcw-338918'
                CLUSTER_NAME = 'cluster-1'
                LOCATION = 'us-central1-c'
                CREDENTIALS_ID = 'docker-hub'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    app = docker.build("erandiranaweera/cloudtest:${env.BUILD_ID}")
                    }
            }
        }
        
        stage('Push image') {
            steps {
                script {
                    withCredentials( \
                                 [string(credentialsId: 'docker-hub',\
                                 variable: 'docker-hub')]) {
                        sh "docker login -u erandiranaweera -p ${docker-hub}"
                    }
                    app.push("${env.BUILD_ID}")
                 }
                                 
            }
        }
    
        stage('Deploy to K8s') {
            steps{
                echo "Deployment started ..."
                sh 'ls -ltr'
                sh 'pwd'
                sh "sed -i 's/pipeline:latest/pipeline:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', \
                  projectId: env.PROJECT_ID, \
                  clusterName: env.CLUSTER_NAME, \
                  location: env.LOCATION, \
                  manifestPattern: 'deployment.yaml', \
                  credentialsId: env.CREDENTIALS_ID, \
                  verifyDeployments: true])
                }
            }
        }    
}

5. Pull the github repository and copy all this file to that directory and push it.

git add .

git commit -m "message"

git push
