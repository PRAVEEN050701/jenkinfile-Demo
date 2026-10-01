pipeline{
    agent any
    parameters{
        string(
            name : 'APP_PORT',
            defaultValue : '3000',
            description : 'Server Port'
        )
    }

    environment{
        IMAGE_NAME='jenkins-demo-app'
    }
    stages{
        stage('checkout'){
            steps{
                echo 'checking out socure code from Git Repo'
                checkout scm
            }
        }
        stage ('Check Docker'){
            steps{
                bat 'docker --version'
            }
        }
        stage('Dependencies'){
            steps{
                bat 'npm install'
            }
        }
        stage('Test APP'){
            steps{
                bat 'npm test'
            }
        }
        stage('build'){
            steps{
                bat 'docker -t %IMAGE_NAME%:%BUILD_NUMBER .'
            }
        }
        stage('Run Container'){
            steps{
                bat '''
                  docker run -d --name node-app-%BUILD_NUMBER% -p %APP_PORT%:3000 %IMAGE_NAME%
                  '''
            }
        }
        stage('verify'){
            steps{
                bat '''
                   echo APP Deployed Sucessfully
                   echo open http://localhost:%APP_PORT%
                   docker ps
                '''
            }
        }
    }
}