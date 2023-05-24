pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                
                echo 'Cloning Git.....'
            }
        }
        stage('Build') {
            steps {
                // Add build steps as necessary
                script {
                echo "Aqui se entra a una EC2 instance del FRONT"
                def sshCommands = [
                        //"sh 'pwd'",
                        "ls",
                        "git https://github.com/Mauriciomery/microservice-app-for-training-devops.git",
                        "pwd",
                        "chmod -R ugo+rwx microservice-app-example/",
                        "curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash",
                        "export NVM_DIR='$HOME/.nvm'"
                        "[ -s '$NVM_DIR/nvm.sh' ] && \. '$NVM_DIR/nvm.sh'",
                        "[ -s '$NVM_DIR/bash_completion' ] && \. '$NVM_DIR/bash_completion'",
                        "nvm install 8.17.0",
                        "cd frontend/",
                        "npm install",
                        "npm audit fix",
                        "npm run build",
                        "PORT=9050 AUTH_API_ADDRESS=http://internal-MM-Internal-LB-2051230687.us-east-1.elb.amazonaws.com:8020 TODOS_API_ADDRESS=http://internal-MM-Internal-LB-2051230687.us-east-1.elb.amazonaws.com:8082 npm start&"
                        // Add more commands as needed
                        "exit"
                    ]
                echo "Aqui se entra a una EC2 instance del FRONT"
                echo "Intentando desde la carpeta de entrenamiento"
                sh 'cd $HOME'   
                sh 'ls'
                def sshConnection = "ssh -t -o StrictHostKeyChecking=no -i 'rampup-mery2.pem' ec2-user@10.0.102.241 '${sshCommands.join(' && ')}'"
                sh sshConnection
                echo 'aqui se salió de la maquina'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing ....'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying project...'
                sh 'JWT_SECRET=PRFT SERVER_PORT=8083 java -jar target/users-api-0.0.1-SNAPSHOT.jar'
            }
        }
    }
}
