pipeline {
  agent any
  stages {
    stage('SCM  Checkout') {
      steps {
        git(url: 'https://gitlab.com/spring-boot-appplication/spring-docker.git', branch: 'master', credentialsId: 'gitlab')
            }
        }
    stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('sonarqube') {
                sh 'mvn clean package sonar:sonar'
              }
            }
        }
    }
}