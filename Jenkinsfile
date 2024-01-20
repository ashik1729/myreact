pipeline {
    agent any
    environment {
        CPANEL_HOST = 'wakra-lab.com'
        CPANEL_USER = 'wakralab'
        CPANEL_PORT = '22'
        LOCAL_BUILD_FOLDER = 'build'
        REMOTE_COPANEL_PATH = '/public_html/myapp'
    }
     environment {
        // Define SSH credentials ID (you should create SSH credentials in Jenkins)
        SSH_CREDENTIALS = 'ssh-credential-agent'
        // Define the remote server details
        REMOTE_SERVER = 'wakra-lab.com'
        REMOTE_USERNAME = 'wakralab'
        REMOTE_COMMAND = 'ls' // Change this to the command you want to execute on the remote server
    }
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'testing react app..'
            }
        }
         stage('Deploy') {
            steps {
                script {
                    // Use the sshagent step to handle SSH authentication
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        // Execute SSH commands on the remote server
                        sh "ssh ${REMOTE_USERNAME}@${REMOTE_SERVER} ${REMOTE_COMMAND}"
                    }
                }
            }
        }

    }
}


