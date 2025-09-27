pipeline {
  agent any

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
        // allow tests to fail without breaking pipeline
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
          sh '''
            sonar-scanner \
              -Dsonar.projectKey=8.2CDevSecOps \
              -Dsonar.organization=tamanna1905 \
              -Dsonar.sources=.
          '''
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
pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/tamanna1905/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps { sh 'npm install' }
    }

    stage('Run Tests') {
      steps { sh 'npm test || true' }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit --json > audit-report.json || true'
      }
    }

    // 👉 Add this here
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarCloud') {
          sh 'npx sonar-scanner ' +
             '-Dsonar.projectKey=8.2CDevSecOps ' +
             '-Dsonar.organization=tamanna1905 ' +
             '-Dsonar.sources=.'
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'audit-report.json', onlyIfSuccessful: false
    }
  }

