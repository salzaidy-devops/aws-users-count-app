pipeline {

    agent any

    stages {
        stage('initialize') {
            steps {
                // checkout scm
                echo 'Initializing...'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm test'
                echo 'Running tests...'
            }
        }
        stage('BuildDockerImage') {
            steps {
                echo 'Building Docker image...'
                // sh 'docker build -t users-count-app:1.0 .'
                // sh 'docker build -t salzaidy/users-count-app:1.0 .'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Docker image...'
                    // we're not doing docker login b/c we've done docker loogin on the EC2 instance already
                    def dockerCMD = 'docker run -d -p 4000:4000 salzaidy/users-count-app:1.0'
                    
                    sshagent(['ec2-server-key']) {
                        // this flag is to avoid host key verification issue
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.17.150.175 ${dockerCMD}"
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}