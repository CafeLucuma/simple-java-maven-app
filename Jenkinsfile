pipeline {
	agent any
    tools {
        sonarscanner 'SonarQube Scanner 3.1'
    }
    stages {
        stage('Build') { 
            agent{
                label 'master'
            }
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }

        stage('Test') {

            agent {
                label 'docker'
            }
            steps {
                sh 'mvn test'
            }
            post {
              always {
                  echo "Escribiendo reporte"
                  junit 'target/surefire-reports/*.xml'
              }
              failure {
                  echo "Test fallido, enviando mail"
              }
          }
      }
      stage('Testing SonarQube'){
        agent {
            label 'linux'
        }
        steps {
            withSonarQubeEnv('SonarQube_Akzio') {
              echo "Sonar scanner path: " + ${SonarScanner}
              sh "${SonarScanner}/bin/sonar-scanner"
          }
      }
  }
  stage("SonarQube Quality Gate") { 
    agent {
        label 'linux'
    }
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

