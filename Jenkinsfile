pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sshagent (credentials: ['6436e7cc-2e12-4a59-ad70-9fa23ef26c03']) {
                    sh 'ssh -o StrictHostKeyChecking=no admin1@192.168.0.158 uname -a'
                    sh 'echo $MACHINE_NAME'

                }
                echo "Running for development"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                echo "Running for production"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
    }
}

