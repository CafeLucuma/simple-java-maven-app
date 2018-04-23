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
    stage('Testing SonarQube'){
        steps {
            def scannerHome = tool 'SonarQube Scanner 2.8';
            withSonarQubeEnv('My SonarQube Server') {
              sh "${scannerHome}/bin/sonar-scanner"
          }
      }
      

  }
  stage('Test') {
     environment {
        DEBUG_FLAGS = '-g'
    }
    steps {
    }
    post {
      always {
          echo "Escribiendo reporte"
          junit '**/target/*.xml'
      }
      failure {
          echo "Test fallido, enviando mail"
      }
  }
}
}
}
