pipeline {
	agent any
    tools {
                maven 'maven' 
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
                label 'linux'
            }
            steps {
                sh 'mvn test'
                echo pwd
                echo 'Is unix: ' + isUnix

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
                    sh 'mvn sonar:sonar ' + 
                    '-Dsonar.junit.reportPaths=target/surefire-reports/'
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

