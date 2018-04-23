pipeline {
	agent any
	environment { 
		CC = 'clang'
		JENKINS_COMMON_CREDS = credentials('jenkins-credentials')
        qg = null
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
  stage('Testing SonarQube'){
    steps {
        withSonarQubeEnv('SonarQube_Akzio') {
            sh 'mvn clean package sonar:sonar ' + 
            '-Dsonar.junit.reportPaths=**/target/*.xml'

        }
    }
}
stage("SonarQube Quality Gate") { 
 options{
    timeout(time: 1, unit: 'HOURS')
}
steps{
    script {
        qg = waitForQualityGate() 
        if (qg.status != 'OK') {
           error "Pipeline aborted due to quality gate failure: ${qg.status}"
       }
   }
   echo "Estado de ĺa quality gate: ${qg.status}"
}
}
}
}
