pipeline {
  agent any

  environment {
    // Java 17 for the scanner
    JAVA_HOME = "/opt/homebrew/opt/openjdk@17"
    PATH = "${JAVA_HOME}/bin:/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Setup SonarScanner') {
      steps {
        sh '''
          set -eux
          SCAN_VER=5.0.1.3006      # macOS build of the current CLI
          mkdir -p .scanner
          cd .scanner

          # Download only if not already cached in workspace
          if [ ! -d "sonar-scanner-${SCAN_VER}-macosx" ]; then
            curl -sSLo sonar-scanner.zip \
              "https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SCAN_VER}-macosx.zip"
            unzip -q sonar-scanner.zip
          fi
        '''
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            set -eux
            .scanner/sonar-scanner-*/bin/sonar-scanner \
              -Dsonar.organization=pramodyasenadheera \
              -Dsonar.projectKey=pramodyasenadheera_8.2CDevSecOps \
              -Dsonar.login=$SONAR_TOKEN
          '''
        }
      }
    }
  }
}
