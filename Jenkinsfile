pipeline {
    agent any
    environment {
        // Define your cPanel server details
        // Define your cPanel server details
        CPANEL_HOST = 'your-cpanel-server.com'
        CPANEL_USER = 'your-ssh-username'
        CPANEL_PORT = '22' // Change this if your SSH server uses a different port
        SSH_PRIVATE_KEY_PATH = 'D:/wakra-lab-ssh/wakralabssh' // Update this to your local SSH private key path
        LOCAL_BUILD_FOLDER = 'build'
        REMOTE_COPANEL_PATH = '/public_html/myapp' // Change this to the desired remote folder path on cPanel
    }
    triggers {
        pollSCM('*/2 * * * *')
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
                echo 'Deploying react app....'
                script {
                    // Copy the contents of the local build folder to the cPanel server
                    sshagent(['ssh-credential-agent']) {
                        sh """
                            scp -o StrictHostKeyChecking=no \
                                -i ${SSH_PRIVATE_KEY_PATH} \
                                -P ${CPANEL_PORT} \
                                -r ${LOCAL_BUILD_FOLDER}/* \
                                ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/
                        """
                    }
                }
            }
        }
    }
}
