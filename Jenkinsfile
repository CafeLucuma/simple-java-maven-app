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
