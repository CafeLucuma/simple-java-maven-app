pipeline {
	agent any
	environment { 
		CC = 'clang'
		JENKINS_COMMON_CREDS = credentials('jenkins-credentials')

	}
	parameters {
        	string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
	}
    stages {
	
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
	stage('Test') {
	    environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
		echo env.CC 
		echo env.DEBUG_FLAGS
                sh 'printenv'
                sh 'make check || true' 
                sh 'mvn test'
		echo 'Testing echo....'
		sh 'ls -l'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
		echo "testing parameters: ${params.Greeting}"
            }
            post {
                post {
			always {
			    junit '**/target/*.xml'
			}
			failure {
			    mail to: oscar.carrasco@akzio.cl, subject: 'The Pipeline failed :('
			}
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
		echo "El test ha sido exitoso $currentBuild.result" 
                sh 'make publish'
            }
        }
    }
}
