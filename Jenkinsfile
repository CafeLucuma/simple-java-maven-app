pipeline {
	agent any
	environment { 
		CC = 'uSED IN ALL STAGES'
	}
    stages {
	
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
	stage('Test') {
            steps {
	    environment {
		LOL = 'Used only in this stage'
                DEBUG_FLAGS = '-g'
            }
            steps {
		echo env.LOL
                sh 'printenv'
            }
                sh 'make check || true' 
                sh 'mvn test'
		echo 'Testing echo....'
		sh 'ls -l'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
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
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
		echo "El test ha fallado $currentBuild.result" 
                sh 'make publish'
            }
        }
    }
}
