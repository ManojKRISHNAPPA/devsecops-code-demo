pipeline {
  agent any

  tools {
    // Make sure Java 17 and Maven are installed/configured in Jenkins tools
    jdk 'jdk-17'
    maven 'maven-3.8.5'
  }

  stages {
    stage('Build and Test') {
      steps {
        script {
          sh 'mvn clean test'
        }
      }
    }

    stage('Mutation Testing') {
      steps {
        script {
          // Run PIT mutation testing, fail build if mutation threshold not met
          sh 'mvn org.pitest:pitest-maven:mutationCoverage'
        }
      }
    }

    stage('Publish PIT Report') {
      steps {
        // Publish PIT mutation testing report (adjust plugin and report path)
        publishHTML(target: [
          reportName: 'PIT Mutation Testing Report',
          reportDir: 'target/pit-reports',
          reportFiles: 'index.html',
          keepAll: true,
          alwaysLinkToLastBuild: true,
          allowMissing: false
        ])
      }
    }
  }

  post {
    always {
      echo 'Cleaning workspace'
      cleanWs()
    }
  }
}
