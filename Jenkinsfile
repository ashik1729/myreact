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
                echo 'Deploying.......'
                script {
                    // Use withCredentials to inject the SSH key
                    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credential-agent', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        // Set environment variable for the SSH private key
                        env.SSH_PRIVATE_KEY = credentials('ssh-credential-agent')
                        sh "ls -a"

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
                        sh "echo 'SSH Config File Content: ${sshConfigFile}'"
                        sh "echo 'SSH Private Key: \${SSH_PRIVATE_KEY}'"
                        sh "ssh -i \${SSH_PRIVATE_KEY} -o StrictHostKeyChecking=no ${CPANEL_USER}@${CPANEL_HOST} 'hostname'"
                        sh "scp -i \${SSH_PRIVATE_KEY} -o StrictHostKeyChecking=no -r ${LOCAL_BUILD_FOLDER}/* ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/"
                        sh "echo 'Deployment completed'"
                    }
                }
            }
        }

    }
}


