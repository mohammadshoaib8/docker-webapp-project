pipeline {
    agent any
    environment {
        IMAGE_NAME = "msshoaib2255457/testdocker"
        IMAGE_TAG = "v${BUILD_NUMBER}"
        
    }
    
    
    stages {
        stage("Git clone") {
            steps {
                git credentialsId: '5344fe9e-333a-4e01-844b-fc56f14330fd', url: 'https://github.com/mohammadshoaib8/docker-webapp-project.git'
            }
        }
        stage("Docker build") {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                    docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }
        stage("Docker push") {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh '''
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
        stage ("Docker Run/Deploy") {
            steps {
                sh '''
                    docker rm -f nginxapp || true
                    docker run --name nginxapp -p 8000:80 -d $IMAGE_NAME:latest
                    docker ps
                    docker images
                '''
            }
        }
    }
}
