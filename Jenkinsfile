pipeline {
    agent any
    triggers {
        pollSCM('*/2 * * * *')
    }
    stages {
        stage('Build') {
            steps {
                echo 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'testing react app..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying react app....'
                echo 'npm install'

            }
        }
    }
}
