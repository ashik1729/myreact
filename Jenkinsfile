pipeline {
    agent any
    environment {
        SSH_CREDENTIALS = 'ssh-credential-agent'
        REMOTE_SERVER = 'wakra-lab.com'
        REMOTE_USERNAME = 'wakralab'
        REMOTE_COMMAND = 'ls'  // Customize for specific deployment tasks
    }
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
        stage('Deploy') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: SSH_CREDENTIALS, keyFileVariable: 'keyfile')]) {
                      //  sh "ssh -i ${keyfile} ${REMOTE_USERNAME}@${REMOTE_SERVER} ${REMOTE_COMMAND}"
                      sh "ssh -oHostKeyAlgorithms=+ssh-rsa -i ${keyfile} wakralab@wakra-lab.com ls"

                    }
                }
            }
        }
    }
}
