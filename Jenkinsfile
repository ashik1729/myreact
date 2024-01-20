pipeline {
    agent any
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
        // stage('Build') {
        //     steps {
        //         sh 'npm install'
        //         sh 'npm run build'
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         echo 'testing react app..'
        //     }
        // }
        stage('Deploy') {
            steps {
                script {
                    // Use the sshagent step to handle SSH authentication
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        // Execute SSH commands on the remote server
                    //  sh 'ssh -i "D:/wakra-lab-ssh/wakralabssh" wakralab@wakra-lab.com "${REMOTE_COMMAND}"'
                                      //sh 'ssh -i "D:\wakra-lab-ssh\wakralabssh" -oHostKeyAlgorithms=ssh-rsa wakralab@wakra-lab.com "${REMOTE_COMMAND}"'
//sh 'ssh -v -i "D:/wakra-lab-ssh/wakralabssh" wakralab@wakra-lab.com "${REMOTE_COMMAND}"'
                sh 'ssh -i "D:/wakra-lab-ssh/wakralabssh" -oHostKeyAlgorithms=ssh-rsa wakralab@wakra-lab.com "${REMOTE_COMMAND}"'


                    }
                }
            }
        }

    }
}


