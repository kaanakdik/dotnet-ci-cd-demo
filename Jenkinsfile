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

        stage('Restore & Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o out'
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
