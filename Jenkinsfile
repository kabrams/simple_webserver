pipeline {
    agent any
    stages {
        stage('Get files')
            steps {
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/docker-compose.yml'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/app.conf'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/app.conf
            }
        }
        stage('certbot_container') {
            agent {
                docker { 
                    image 'certbot/certbot'
                    args '-v ./data/certbot/conf:/etc/letsencrypt'
                    args '-v ./data/certbot/www:/var/www/certbot'
                }
            }
            steps {
                echo 'Building certbot container..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying..'
            }
        }

    }

}

