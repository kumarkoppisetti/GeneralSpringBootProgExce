pipeline {
  agent any
 
  environment {
    SONAR_TOKEN = credentials('sonarqube')  // 🔐 Fetch securely
  }
 
  stages {
    stage('Build') {
      steps {
        git branch: 'development', url: 'https://github.com/kumarkoppisetti/GeneralSpringBootProgExce'
        sh 'mvn clean package'
      }
    }
 
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('sonarqube') {  // 🔍 Must match name set in Jenkins config
          sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
        }
      }
    }
 
    stage('Quality Gate') {
      steps {
        timeout(time: 15, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
