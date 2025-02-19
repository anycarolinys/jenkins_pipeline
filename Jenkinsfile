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

        // stage('Install Docker') {
        //     steps {
        //         script {
        //             sh '''
        //                 apt-get update

        //                 apt-get install -y ca-certificates curl

        //                 install -m 0755 -d /etc/apt/keyrings

        //                 curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

        //                 chmod a+r /etc/apt/keyrings/docker.asc

        //                 echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

        //                 apt-get update

        //                 apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin

        //                 docker --version
        //             '''
        //         }
        //     }
        // }
        
        //  stage('Install Docker Compose') {
        //     steps {
        //         script {
        //             sh '''
        //                 DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
        //             '''
        //             sh '''
        //                 mkdir -p $DOCKER_CONFIG/cli-plugins
        //             '''
        //             sh '''
        //                 curl -SL https://github.com/docker/compose/releases/download/v2.32.4/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
        //             '''
        //             sh '''
        //                 chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
        //             '''
        //             sh '''
        //                 export PATH=$PATH:$DOCKER_CONFIG/cli-plugins
        //             '''
        //             sh '''
        //                 $DOCKER_CONFIG/cli-plugins/docker-compose --version
        //             '''
        //         }
        //     }
        // }
        
        stage('Stop mysql and flaskapi containers') {
            steps {
                sh '''
                    docker ps -a -q --filter name=mysql_db --filter name=flask_api | xargs -r docker rm -f
                '''
            }
        }

        // stage('Deploy - Run Flask Project') {
        //     steps {
        //         sh '''
        //             $DOCKER_CONFIG/cli-plugins/docker-compose up --build -d
        //         '''
        //     }
        // }

        stage('Create virtual environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
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
