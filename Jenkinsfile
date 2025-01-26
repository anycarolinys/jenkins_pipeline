pipeline {
    agent any

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/anycarolinys/dockerized_flask_api.git'
            }
        }

        stage('List Files') {
            steps {
                sh 'ls -la'
            }
        }


        stage('Install Docker') {
            steps {
                script {
                    sh '''
                        apt-get update

                        apt-get install -y ca-certificates curl

                        install -m 0755 -d /etc/apt/keyrings

                        curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

                        chmod a+r /etc/apt/keyrings/docker.asc

                        echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

                        apt-get update

                        apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin

                        docker --version
                    '''
                }
            }
        }
        
         stage('Install Docker Compose') {
            steps {
                script {
                    sh '''
                        DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
                    '''
                    sh '''
                        mkdir -p $DOCKER_CONFIG/cli-plugins
                    '''
                    sh '''
                        curl -SL https://github.com/docker/compose/releases/download/v2.32.4/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
                    '''
                    sh '''
                        chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
                    '''
                    sh '''
                        export PATH=$PATH:$DOCKER_CONFIG/cli-plugins
                    '''
                    sh '''
                        $DOCKER_CONFIG/cli-plugins/docker-compose --version
                    '''
                }
            }
        }
        
        stage('Stop mysql and flaskapi containers') {
            steps {
                sh '''
                    docker ps -a -q --filter name=mysql_db --filter name=flask_api | xargs -r docker rm -f
                '''
            }
        }

        stage('Docker build .') {
            steps {
                sh '''
                    docker build .
                '''
            }
        }

        stage('Deploy - Run Mysql Container') {
            steps {
                sh '''
                    $DOCKER_CONFIG/cli-plugins/docker-compose up -d db
                '''
            }
        }

        stage('Deploy - Run Flask Project') {
            steps {
                sh '''
                    $DOCKER_CONFIG/cli-plugins/docker-compose up -d flask_api
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
        
        stage('Delay Before Command') {
            steps {
                script {
                    sleep(time: 30, unit: 'SECONDS')
                }
            }
        }
        
        stage('Docker logs') {
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
        
        stage('Docker ps 2') {
            steps {
                sh '''
                    docker ps
                '''
            }
        }
    }
}
