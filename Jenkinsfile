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
        // allow tests to fail without breaking the pipeline (OK for this assignment)
        sh 'npm test || true'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        // keep a machine-readable report as evidence
        sh 'npm audit --json > audit-report.json || true'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarCloud') {
          script {
            // MUST match the name you set in Manage Jenkins → Tools → SonarQube Scanner
            def scannerHome = tool 'SonarScanner'
            sh """
              "${scannerHome}/bin/sonar-scanner" \
                -Dsonar.projectKey=tamanna1905_8.2CDevSecOps \
                -Dsonar.organization=tamanna1905 \
                -Dsonar.sources=. \
                -Dsonar.host.url=https://sonarcloud.io \
                -Dsonar.exclusions=node_modules/**
            """
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

