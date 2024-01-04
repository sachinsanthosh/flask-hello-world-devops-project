pipeline {
    agent any
    
    environment {
        DOCKER_HUB_REPO = "sachinhub1296/flask-hello-world"
        CONTAINER_NAME = "flask-hello-world"
        DOCKERHUB_CREDENTIALS=credentials('docker-cred')
    }
    
    stages {
        /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */
        
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
                sh 'docker run --name $CONTAINER_NAME $DOCKER_HUB_REPO /bin/bash -c "pytest test.py && flake8"'
            }
        }
        stage('Push') {
            steps {
                echo 'Pushing image..'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_HUB_REPO:latest'
            }
        }
        stage('Deploy') {
           steps {
                echo "Deploying the container"
                sh "docker run -d -p 4444:4444 sachinhub1296/flask-hello-world"
                
            }
        }
    }
}
