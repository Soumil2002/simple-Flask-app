pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-docker-app'
        CONTAINER_NAME = 'flask-container'
        PORT = '5000'
    }
   
    stages {
        stage('Clone from GitHub') {
            steps {
                echo 'Cloning from GitHub...'
                git branch: 'main', url: 'https://github.com/Soumil2002/simple-Flask-app.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Mysonar') {
                    sh '/opt/sonar-scanner/bin/sonar-scanner'
                }
            }
        }
        stage('Build Docker image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Stop & Remove Existing Container') {
            steps {
                echo 'Stopping and removing old container if it exists...'
                sh '''
                    if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                        docker stop $CONTAINER_NAME || true
                        docker rm $CONTAINER_NAME || true
                    fi
                '''
            }
        }
        stage('Run Docker image') {
            steps {
                echo 'Running Docker container...'
                sh 'docker run -d -p $PORT:5000 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }
}
