pipeline {
	agent any
    tools {
        maven 'maven' 
    }
    environment {
        qg = null
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
            script {
                echo "Inicializando sonar"
                def scanner = tool 'SonarScanner';
                withSonarQubeEnv('SonarQube_Akzio') {
                  echo "Sonar scanner path: " + scanner
                  sh "${scanner}/bin/sonar-scanner"
              }
          }

          echo "Sonar terminado"

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

