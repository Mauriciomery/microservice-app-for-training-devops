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
                        "git clone https://github.com/Mauriciomery/microservice-app-for-training-devops.git",
                        "pwd",
                        "chmod -R ugo+rwx microservice-app-for-training-devops",
                        "nvm install 8.17.0",
                        "cd microservice-app-for-training-devops/",
                        "cd frontend/",
                        "npm install",
                        "npm audit fix",
                        "npm run build",
                        "PORT=9050 AUTH_API_ADDRESS=http://internal-MM-Internal-LB-2051230687.us-east-1.elb.amazonaws.com:8020 TODOS_API_ADDRESS=http://internal-MM-Internal-LB-2051230687.us-east-1.elb.amazonaws.com:8082 npm start &",
                        "exit"
                    ]
                echo "Aqui se entra a una EC2 instance del FRONT"
                echo "Intentando desde la carpeta de entrenamiento"
                sh 'cd $HOME'   
                sh 'ls'
                def sshConnection = "ssh -t -o StrictHostKeyChecking=no -i 'rampup-mery2.pem' ec2-user@10.0.101.177 '${sshCommands.join(' && ')}'"
                sh sshConnection
                echo 'aqui se salió de la maquina'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing ....'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying project succesful'

            }
        }
    }
}
