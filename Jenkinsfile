pipeline{
    agent any

    tools{
        nodejs 'node18'
    }

    environment{
        MONGO_URI = ''
    }

    stages{
        stage("Clean Workspace"){
            steps{
                echo "=====Cleaning Workspace====="
                cleanWs()
            }
        }

        stage("Checkout code"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Rippyblogger/Trackit-e2e-pipeline.git'
            }
        }

        stage("Install Client and ServerDependencies"){
            steps{
                dir('client'){
                sh 'npm install'
                }

                 dir('server'){
                sh 'npm install'
                }
            }
        }

        stage("Install Client and Server dependencies"){
            steps{
                dir('client'){
                sh 'npm install'
                }

                 dir('server'){
                sh 'npm install && cp .env.EXAMPLE .ENV'
                }
            }
        }

        stage("Setup CI mongo DB and set MONGO_URI env"){
            steps{
                script{
                    sh 'docker run --name mongo -d -p 27017:27017 mongo:latest'
                    sh 'sleep 5'
                    def mongoIpAddress = sh (
                        script: "docker ps -aq | xargs docker inspect --format '{{ .NetworkSettings.IPAddress }}' ",  
                        returnStdout: true
                    ).trim()
                    MONGO_URI  = 'mongodb://$mongoIpAddress:27017'
                    sh "echo '${MONGO_URI}' "
                }
            }
        }

        stage("Clean Mongo container"){
            steps{
                sh 'docker stop mongo'
                sh 'docker rm mongo'
            }
        }
            
    }
}