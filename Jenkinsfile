pipeline {
	agent any
	environment { 
		CC = 'clang'
		JENKINS_COMMON_CREDS = credentials('jenkins-credentials')

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
    stage("SonarQube Quality Gate") { 
       options{
        timeout(time: 1, unit: 'HOURS')
        steps{
           def qg = waitForQualityGate() 
           if (qg.status != 'OK') {
             error "Pipeline aborted due to quality gate failure: ${qg.status}"
         }
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
