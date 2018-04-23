pipeline {
	agent any
    stages {
	
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
	stage('Test') {
            steps {
                sh 'make check || true' 
                sh 'mvn test'
		echo 'Testing echo....'
		sh 'ls -l'
		echo Container.id || true
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
	            echo currentBuild.result
                }
            }
        }
	stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'FAILURE' 
              }
            }
            steps {
		echo "El test ha fallado $currentBuild.result" 
                sh 'make publish'
            }
        }
    }
}
