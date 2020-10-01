pipeline {
    agent any
    environment {
        PATH = "$PATH:/usr/local/bin"
        registry = "kabrams17/simple_web_server"
        registry_url = "https://hub.docker.com/repository/docker/kabrams17/simple_web_server"
        registryCredential = 'docker-creds'
        dockerImage = ''
        currentImage="simplewebserverpipeline_nginx_1"
    }
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
                sh './init-letsencrypt.sh'
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
                sh 'docker cp android-chrome-192x192.png simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp android-chrome-512x512.png simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp apple-touch-icon.png simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon-16x16.png simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon-32x32.png simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp favicon.ico simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp index.html simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp resume.html simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'docker cp site.webmanifest simplewebserverpipeline_nginx_1:/usr/share/nginx/html/'
                sh 'sudo mv app.conf ./data/nginx'
                echo 'Files moved over to correct location in docker container'
            }   
        }
        stage('Create custom image') {
            steps {
                sh 'docker commit ${currentImage} new_webserver'        
            }
        }
        stage('Push image to dockerhub') {
            steps {
                script {
                    docker.withRegistry( '' , registryCredential) {
                        sh 'docker push new_webserver'
                    }
                }
            }
        }
    }
 }

    

