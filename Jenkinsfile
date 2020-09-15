pipeline {
    agent any
    stages {
        stage('Get files') {
            steps {
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/docker-compose.yml'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/app.conf'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/init-letsencrypt.sh'
                sh 'chmod +x init-letsencrypt.sh'
                echo 'Copied over files'
            }
        }
        stage('Run letsencrypt script') {
            steps {
                echo 'Running letsencrypt script..'
                sh 'sudo ./init-letsencrypt.sh'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying..'
                sh 'docker-compose up -d'
            }
        }
        stage('Get website pages') {
            steps {
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/android-chrome-192x192.png'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/android-chrome-512x512.png'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/apple-touch-icon.png'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/favicon-16x16.png'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/favicon-32x32.png'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/favicon.ico'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/index.html'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/resume.html'
                sh 'curl -LJO https://raw.githubusercontent.com/kabrams/simple_webserver/master/site.webmanifest'
                echo 'Copied over website files'
            }
        }
        stage('Move files over to directory') {
            steps {
                sh 'docker cp android-chrome-192x192.png jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp android-chrome-512x512.png jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp apple-touch-icon.png jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon-16x16.png jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon-32x32.png jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon.ico jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp index.html jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp resume.html jenkins-data_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp site.webmanifest jenkins-data_nginx_1:/usr/share/nginx/html/'
            }   
        }
    }
}

