pipeline {
    agent any
    environment {
        // ... (other environment variables)
    // Define SSH credentials ID (you should create SSH credentials in Jenkins)
        SSH_CREDENTIALS = 'ssh-credential-agent'
        // Define the remote server details
        REMOTE_SERVER = 'wakra-lab.com'
        REMOTE_USERNAME = 'wakralab'
        REMOTE_COMMAND = 'ls' // Change this to the command you want to execute on the remote server
        PRIVATE_KEY_CONTENT = """-----BEGIN RSA PRIVATE KEY-----
MIIEoAIBAAKCAQEA4cTn9GSZ2EQyBCwhbgG4H/WiZShvNbmBFX92kYQilJkqYGoU
hdilnv1s5brAvKV4/y8fBk091Uhi3wvDrYsFC3mf3erxVBzZUNvdjTyNhjSFYguX
KQHpdxuSQFidwcVUPbNSnAwkLQ06nXlzaiCSrlLlSRD6KVYm6Ocl5LJdwhVvktqo
tSUYXBsknxc9B/wtOYjGeKxITXMx0FxQgQN9YCQ0wGZpGZUSDl07LTq4YDtNEMfw
wmhe4QPmEC6Xf54R1+aeke/TC6wnI9iAYOPgEZnq25FanpuWkGyPXeRwnJk+g6S/
mzP3yTDu4RGUfNQZAOjsl7uhi95aqZteGUzCHwIBIwKCAQBtqMhvZBA11/sJV0Nv
8jTcUryXiKsLd1v0f70/XW/hxrzPvn7/LrbYMfMQf0esFduvJYQZAPIlxBLEG6gv
uI1r+T8FY3yH8MFTKPaVDshlwb0g/lC8JYAGpv3kvVPwZyjqxNBabEwV4dqkQk4A
WPbJszwqzbtV6ARFPRJnxFls5O2EPH6v8nb31XBz+vkNGtOQ+InSmoRnVyXpHnmE
/cl8w+9YmUqmfxnkBjzjOGOANJXR6BCyHvZ1d0twh8+tPpeWitklQYhyNCSPaSUy
GehqUzPkQVdy1Fh8M7fvsUGC8AIC48gxGxWZFr6vZXQCQ2MSFkEW9GzrNjvkNebT
gOFDAoGBAPVq9MmT2Rvr6bAUFgropSSUU2Uuz8/gwkqknyplkqbbv31W+bvWtw2q
sVMn0tPZtuItOl9n5oCyuFOdpncLhezUlgFZZjWD9xhyXpxh/SVO3CocZC3AIieu
qLcYyr7Na3efBmwZhm3cgJF3eAH03lCfuX6ceyRR4E7Uo7/V/k1jAoGBAOuBDvhr
wjSuLroyz0981II+gPo96aQQZuRp59GRKhmSlRXvQCKgyUeONabmTia6zNwVRl/r
yaaHv2CK2e1atWVj9oPNt2Z+duGKOADT/K8VA2T4TEumxEA8aGyi6P3Bmf78yVz+
dcberra1Mhwo8pL/tlFilI3sUicdPVme6sMVAoGAVCSrstOa5QBtYPDxnVcUDIqg
P/LK6C/Nle9Mg5CKDVKoDbdrkNvm/V8YOcSCzEqlGlimTJimzQoTT95Hs8lvvunN
B8bhNutqqUvIqqVA2ZdhbYYTt+oaVr+KTWeV+EZt/SCUfNWNLPskxCj18grP4SDR
4kRHeiqzTjpGxW3wyhMCgYB/2FiVfFN7rwNsZLmuzryBN9+dydaa5FUc6QjQ2cZl
p1g/G3qWdIqF7h0gFp90HENS5vpRVB0CoXaE3amA2XhqPZRzb6yz+4JdLcahXSLF
cc6khspU9CFksxQdt4XLlPvTkIqRkXMi0Kf8yMNuX18cmXGZ5Q7Qs30N600wprn0
3wKBgFrhxfrln+qDztbH4LqB40gU3jOFKxmZOG0cJGdJ4ZMKxm2/EakKv6Zh4mRX
dQeEtARdZ48xk0p0BZi6Ke1GZYzfG1nxygSjNGDA9ClM4oDnr7CBcBbGepQQtMLe
W2mXq0tYCYt5mZk4qlAZB7lNV/yOktL9cBjs/kpaZr0SFa1d
-----END RSA PRIVATE KEY-----"""
    }
    stages {
        stage('Deploy') {
            steps {
                script {
                    // Save private key to a file with restricted permissions
                    writeFile file: 'private_key.pem', text: PRIVATE_KEY_CONTENT
                    sh 'chmod 600 private_key.pem'

                    // Use the sshagent step to handle SSH authentication
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        // Execute SSH commands on the remote server
                        sh 'ssh -i private_key.pem -oHostKeyAlgorithms=ssh-rsa ${REMOTE_USERNAME}@${REMOTE_SERVER} "${REMOTE_COMMAND}"'
                    }
                }
            }
        }
    }
}
