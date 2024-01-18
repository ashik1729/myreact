pipeline {
    agent any
    environment {
        // Define your cPanel server details
        // Define your cPanel server details
        CPANEL_HOST = 'wakra-lab.com'
        CPANEL_USER = 'wakralab'
        CPANEL_PORT = '22' // Change this if your SSH server uses a different port
        // SSH_PRIVATE_KEY_PATH = 'D:/wakra-lab-ssh/wakralabssh' // Update this to your local SSH private key path
        SSH_PRIVATE_KEY = """
           -----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,DF22A90E3D0895E4

YCltH0IADItWzw8aGapEvTJac4/XhYg6ADP+9Dhtiys4od6xKF3HAC2IAVZJatIE
qo66hgl869hGcA8fM+44uTVzBwF6mBnn5SkfeVLePYWysgZJS//KjvLxLFqgb3qO
MiRmA7JnwP2IPsz68SReIGmJZPyA5QVh0p3DC8dRQKUG2sjTK9+cL8kG2oBnHyWY
C1xJOm7eXr6dTb9zjA9ph3WJDi6CiDygCfJitPshubp73OpGiNa+ehbIdh1aoFvA
DpoU1xbKPNoW0L6JdBrQR8rluTjhn2swoHld9DYXfni2Qmh4ADgZ81yUcrAhtJpa
EWpQrvopVNvtheF58/pwA/loK7vN7roUu9rtihYhNl2CZHpkVP8w7kglaMAHLqZR
8pFOT+gwEvT+dDkt5WNvOz/32whDKtwKKTqOeJ8yBpqmn5Md18oQASYAMxY0e9k9
I6OPTEQN+PWSo6mBDSiWI9wKztGwpgeM0EGiwuvW2CyVEl5wSy50XEvHI0Bbtzg3
jNC5gwSQl3QUHi0wn1vSe8orMT7qrDQWZ55DUOGCBPDeEHX5fW3UYV96Mzo/y6Xe
xwYrVWlDuCPMO9Iho4Gp7lwlTtMfL95Oc4QYcOi8qJbHtHfqtGTZwJssbuIEC6M4
l4iqG8UAIkZ0lEhbEWwJafqyN/Vt4dHQkq0qVto9I7/gBkgZIcgf4ZWgAsSdvHaE
yDnQK0RfqPg9bZPw3VqRxheySvbFbtphQ+NzqAWdDZyjUDTSTmJgNbEl1MrLB+dK
EkZYKAfQJX/risrzWSs2Uec1xt2+0s/8FZu/OTYwQhL86sFHgNWlKVLrVIj7khiU
dMCEl2lcSe5X5t/TNPOvm2P33L+e/j+cDQD960SEPQCgXuFqnW/O0vsQjCsCE5BT
mD7cVtKOiXr9tT9/yGIZiyrKvbZJEqHufRU+J8z/ZrdgTLmqie3zJYwbsv2YCc91
tjHc9/D0vvKFeO/bzuwUin5lyco89GI+vDkz85bRz02M8ObPVWKpmyR+bGbO0dCP
nQIRHahYFVyOLlcQ71LJTWYbuHNFsivuNExlDOqC9K7ZpBG8cxs92R1Pt/z1OAyX
o2JJ4uP5ZiECnHaIyM0kV2FAhXtuyQxpXFa/fUX+LLtxVxX5zM+fMIvC8VZALnQl
IhLSs/C+tqEwOIsFgDg6XJsUxnAzWT/zev2Q2oXIVAT2DSuVm8kj8FZNBqfQHHF2
ZSsITxh/jeeQYsIAtmnCDgLd+L51C8B7EUOarCAEpxoxK4HTm8TWWqDaKr8WB6Qn
nOAqGvpMNy2tRoEgk6K/iAM3XjW1bNCs5roJjd2wh8XVT4fcilbacutSW1zI4m1d
KG26v+NGSGf8xlGda2Ms7MdrXMweOHjwLyJptCwYjpfmHSVmz7LPXmoI6lFgvkhH
vBtIFARfaDjrwSYpLWKO2a+C4IzJYh/TifYLAJ8KUA7IaURNJ4tXS78PBTcsu+cX
AlrkTTRYPk6wujIWsiTaveXcVUmEgQVSHh5mcc2RcGHD5bweYUswq16VpMr/xsKe
vi2TT2u1izxfYb37vqCH2ALcmuY6dba2s9vPOaD0G6P0WLH7D7iMTA==
-----END RSA PRIVATE KEY-----
        """
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
                // script {
                //     // Copy the contents of the local build folder to the cPanel server
                //     sshagent(['ssh-credential-agent']) {
                //         sh """
                //             scp -o StrictHostKeyChecking=no \
                //                 -i ${SSH_PRIVATE_KEY_PATH} \
                //                 -P ${CPANEL_PORT} \
                //                 -r ${LOCAL_BUILD_FOLDER}/* \
                //                 ${CPANEL_USER}@${CPANEL_HOST}:${REMOTE_COPANEL_PATH}/
                //         """
                //     }
                // }
                 script {
                    // Use withCredentials to inject the SSH key
                    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credential-agent', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        sh """
                            scp -o StrictHostKeyChecking=no \
                                -i ${SSH_PRIVATE_KEY} \
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
