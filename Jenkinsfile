pipeline {
    agent any
    environment {
        // Use the credential ID for SSH private key
        SSH_CREDENTIALS = 'ssh-credential-agent'
        REMOTE_SERVER = 'wakra-lab.com'
        REMOTE_USERNAME = 'wakralab'
        REMOTE_COMMAND = 'ls'
    }
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
        stage('Deploy') {
            steps {
                script {
                    // Use the sshagent step to handle SSH authentication
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        sh 'ssh -oHostKeyAlgorithms=ssh-rsa -l wakralab wakra-lab.com "${REMOTE_COMMAND}"'
                    }
                }
            }
        }
    }
}
