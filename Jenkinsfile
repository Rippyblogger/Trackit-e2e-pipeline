pipeline{
    agent any

    tools{
        nodejs 'node18'
    }

    environment{
        MONGO_DB_CONNECTION = ''
        SECRET_KEY = credentials('SECRET_KEY')
        PASSWORD_SALT = credentials('PASSWORD_SALT')
        JWT_TOKEN_EXPIRATION = credentials('JWT_TOKEN_EXPIRATION')
        NODE_ENV= 'production'
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

        stage("Setup CI mongo DB and set MONGO_DB_CONNECTION env"){
            steps{
                script{
                    sh 'docker run --name mongo -d -p 27017:27017 mongo:latest'
                    sh 'sleep 5'
                    def mongoIpAddress = sh (
                        script: "docker ps -aq | xargs docker inspect --format '{{ .NetworkSettings.IPAddress }}' ",  
                        returnStdout: true
                    ).trim()
                    MONGO_DB_CONNECTION  = "mongodb://${mongoIpAddress}:27017"
                }
            }
        }

        stage("Run Tests"){
            steps{
                dir("server"){
                    sh 'touch .env'
                    sh """
                        cat <<EOF > .env
                        MONGO_DB_CONNECTION=${MONGO_DB_CONNECTION}
                        SECRET_KEY=${SECRET_KEY}
                        PASSWORD_SALT=${PASSWORD_SALT}
                        JWT_TOKEN_EXPIRATION=${JWT_TOKEN_EXPIRATION}
                        NODE_ENV=${NODE_ENV}
                        EOF
                    """

                    sh 'npm run test'
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