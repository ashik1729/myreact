pipeline {
    agent any
    environment {
        CPANEL_HOST = 'wakra-lab.com'
        CPANEL_USER = 'wakralab'
        CPANEL_PORT = '22'
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
                echo 'Testing React app...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                script {
                    // Use withCredentials to inject the SSH key
                    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credential-agent', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        // Set environment variable for the SSH private key
                        env.SSH_PRIVATE_KEY = credentials('ssh-credential-agent')
                        
                        // Create a temporary SSH configuration file using writeFile
                        def sshConfigFile = """Host ${CPANEL_HOST}
    HostName ${CPANEL_HOST}
    Port ${CPANEL_PORT}
    User ${CPANEL_USER}
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    PubkeyAcceptedKeyTypes +ssh-rsa
"""
                        def configFile = writeFile file: 'ssh-config', text: sshConfigFile
                        echo "SSH Config File Content: ${sshConfigFile}"
                        echo "SSH Private Key: ${SSH_PRIVATE_KEY}"

                        // SSH command to check the connection and list items in the remote location
                        def sshCommand = """
                            ssh -i ${SSH_PRIVATE_KEY} -o StrictHostKeyChecking=no ${CPANEL_USER}@${CPANEL_HOST} 'hostname; ls ${REMOTE_COPANEL_PATH}'
                        """
                        def sshResult = sh(script: sshCommand, returnStatus: true)

                        if (sshResult == 0) {
                            echo 'SSH connection established successfully'
                        } else {
                            error 'Failed to establish SSH connection'
                        }

                        // Continue with deployment steps
                        sh "scp -i ${SSH_PRIVATE_KEY} -o StrictHostKeyChecking=no -r ${LOCAL_BUILD_FOLDER}/* ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/"
                        echo 'Deployment completed'
                    }
                }
            }
        }
    }
}
