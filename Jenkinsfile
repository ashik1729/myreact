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

                        // Create a temporary SSH configuration file
                        def sshConfigFile = """Host ${CPANEL_HOST}
                            HostName ${CPANEL_HOST}
                            Port ${CPANEL_PORT}
                            User ${CPANEL_USER}
                            StrictHostKeyChecking no
                            UserKnownHostsFile /dev/null
                            PubkeyAcceptedKeyTypes +ssh-rsa
                        """

                        // Write the configuration to a temporary file
                        def configFile = writeTmpFile(sshConfigFile)

                        // Use the temporary SSH configuration file in the scp command
                        sh "scp -F ${configFile} -i \${SSH_PRIVATE_KEY} -r ${LOCAL_BUILD_FOLDER}/* ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/"

                        // Cleanup the temporary file
                        sh "rm ${configFile}"
                    }
                }
            }
        }
    }
}

def writeTmpFile(contents) {
    def tmpFile = File.createTempFile("ssh-config", null)
    tmpFile.text = contents
    tmpFile.setExecutable(true)
    return tmpFile
}
