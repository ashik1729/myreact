pipeline {
    agent any
    environment {
        CPANEL_HOST = 'wakra-lab.com'
        CPANEL_USER = 'wakralab'
        CPANEL_PORT = '22'
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

                        // Echo a message indicating that the SSH connection has been established
                        echo 'SSH connection established'

                        // Use the temporary SSH configuration file in the ssh command
                        sh "ssh -F ${configFile} -i \${SSH_PRIVATE_KEY} ${CPANEL_USER}@${CPANEL_HOST} 'ls -l'"
                    }
                }
            }
        }
    }
}
