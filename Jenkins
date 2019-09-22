node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get code from a GitHub repository
      git 'https://github.com/vbansal32/sample-eureka.git'
      mvnHome = tool 'localMaven'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('SonarQube Analysis') {
        def mvnHome1 = tool name: 'localMaven', type: 'maven'
        withSonarQubeEnv('sonarqube'){
            sh "${mvnHome1}/bin/mvn sonar:sonar"   
        }
    }
   stage('Docker Build & Push') {
      // Build docker image and push to dockerhub
      echo pwd()
      docker.withRegistry('http://130.211.120.48:8083', 'nexusDockerRegistry') {
        def customImage = docker.build("eureka-service:${env.BUILD_ID}")
        /* Push the container to the custom Registry */
        customImage.push()
      }
   }
}
