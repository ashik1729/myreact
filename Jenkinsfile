pipeline {
    agent any
    environment {
        CPANEL_HOST = 'wakra-lab.com'
        CPANEL_USER = 'wakralab'
        LOCAL_BUILD_FOLDER = 'build'
        REMOTE_COPANEL_PATH = '/public_html/myapp'
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
                    // Use withCredentials to inject the SSH key
                    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credential-agent', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        // Set environment variable for the SSH private key
                        env.SSH_PRIVATE_KEY = credentials('ssh-credential-agent')

                        // Use environment variable in the scp command
                        sh """
                            scp -o StrictHostKeyChecking=no \
                                -i \${SSH_PRIVATE_KEY} \
                                -r ${LOCAL_BUILD_FOLDER}/* \
                                ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/
                        """
                    }
                }
            }
        }
    }
}
