pipeline{
    agent any

    tools{
        nodejs 'node18'
    }

    environment{
        PLACEHOLDER = "placeholder Value"
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

        stage("Install Client Dependencies"){
            steps{
                dir('client'){
                sh 'npm install'
            }
            }
        }
    }
}