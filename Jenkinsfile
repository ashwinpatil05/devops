#!groovy​

node {
   def mvnHome = tool 'maven'

   stage('Prepare') {
      git url: 'git@github.com:ashwinpatil05/devops.git', branch: 'develop'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
      }
   }
   stage('Deploy') {
       sh 'curl -u jenkins:jenkins -T target/**.war "http://localhost:8080/manager/text/deploy?path=/devops&update=true"'
   }
   stage("Smoke Test"){
       sh "curl --retry-delay 10 --retry 5 http://localhost:8080/devops"
   }
}
