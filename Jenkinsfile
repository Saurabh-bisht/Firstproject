pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/Saurabh-bisht/Firstproject']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'cicd', keyFileVariable: 'demo', passphraseVariable: '', usernameVariable: 'ec2-user')]) {
                    sshagent(['cicd']) {
                        sh "ssh -o StrictHostKeyChecking=no -i $demo $ec2-user@100.27.44.158 'sudo systemctl stop test.service'"
                        sh "scp -i $SSH_KEY target/your-app.jar $ec2-user@100.27.44.158:/tmp/"
                        sh "ssh -o StrictHostKeyChecking=no -i $demo $ec2-user@100.27.44.158 'sudo mv /tmp/test.jar /opt/test/ && sudo systemctl start your-app.service'"
                    }
                }
            }
        }
    }
}
