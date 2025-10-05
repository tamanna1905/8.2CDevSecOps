pipeline {
  agent any

  environment {
    SONAR_SCANNER_OPTS = "-Xmx512m"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/tamanna1905/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test || true'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit --json > audit-report.json || true'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarCloud') {
          withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
            script {
              def scannerHome = tool 'SonarScanner'
              sh """
                "${scannerHome}/bin/sonar-scanner" \
                  -Dsonar.projectKey=tamanna1905_8.2CDevSecOps \
                  -Dsonar.organization=tamanna1905 \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=https://sonarcloud.io \
                  -Dsonar.login=$SONAR_TOKEN \
                  -Dsonar.exclusions=node_modules/**
              """
            }
          }
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'audit-report.json', onlyIfSuccessful: false
    }
  }
}


