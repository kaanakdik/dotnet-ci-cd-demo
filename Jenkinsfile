pipeline {
    agent any

    environment {
        IMAGE_NAME = "dotnet-ci-cd-api"
        CONTAINER_NAME = "dotnet-api-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/kaanakdik/dotnet-ci-cd-demo.git'
            }
        }

        stage('Build in Docker') {
            steps {
                script {
                    sh '''
                        docker run --rm -v $(pwd):/app -w /app mcr.microsoft.com/dotnet/sdk:8.0 \
                        /bin/bash -c "dotnet restore && dotnet build -c Release && dotnet publish -c Release -o out"
                    '''
                }
            }
        }

         stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }       

        stage('Docker Run') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 8085:80 $IMAGE_NAME
                '''
            }
        }
    }
}
