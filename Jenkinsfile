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
            withSonarQubeEnv('SonarQube_Akzio') {
                sh 'mvn clean package sonar:sonar'
          }
      }


  }
  stage('Test') {
     environment {
        DEBUG_FLAGS = '-g'
    }
    steps {
        echo "Step"
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
