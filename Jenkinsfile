pipeline {
    agent any

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/anycarolinys/flask_api_for_jenkins.git'
            }
        }

        stage('List Files') {
            steps {
                sh 'ls -la'
            }
        }
        
        stage('Checking Linux distro') {
            steps {
                sh 'cat /etc/os-release'
            }
        }
        
        stage('Install Docker and Docker Compose') {
            steps {
                sh '''apt-get update -y && apt-get upgrade -y
                apt install docker.io -y
                apt install docker-compose -y
                '''
            }
        }
        
        stage('Stop mysql and flaskapi containers') {
            steps {
                sh '''
                    docker ps -a -q --filter name=mysql_db --filter name=flask_api | xargs -r docker rm -f
                '''
            }
        }

        stage('Install venv') {
            steps {
                sh '''
                    apt install python3.11-venv -y
                '''
            }
        }

        stage('Create virtual environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                '''
            }
        }

        stage('Deploy - Run Flask Project') {
            steps {
                sh '''
                    docker-compose up -d
                '''
            }
        }

        stage('Docker ps') {
            steps {
                sh '''
                    docker ps
                '''
            }
        }
        
        stage('Docker logs for flask_api') {
            steps {
                sh '''
                    docker logs flask_api
                '''
            }
        }

        
        stage('POST to Flask API') {
            steps {
                sh '''
                    curl -X POST http://localhost:5000/api/users \
                    -H "Content-Type: application/json" \
                    -d '{"name": "Any", "birth_date": "1990-05-15"}'

                '''
            }
        }
        
        stage('Docker logs for flask_api after request') {
            steps {
                sh '''
                    docker logs flask_api
                '''
            }
        }
    }
}
