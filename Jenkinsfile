pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sshagent (credentials: ['07f7c5fc-81d1-4d28-a966-f8fafad37aee']) {
                    sh 'ssh admin1@192.168.0.158'
                    sh 'echo $MACHINE_NAME'
                    sh 'ls'
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

