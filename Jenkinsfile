pipeline {
  agent any

  environment {
    // macOS + Homebrew Java 17
    JAVA_HOME = "/opt/homebrew/opt/openjdk@17"
    PATH = "${JAVA_HOME}/bin:/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
    SONAR_TOKEN = credentials('SONAR_TOKEN')   // your Jenkins secret
    SONAR_SCANNER_VERSION = "5.0.1.3006"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git'
      }
    }

    stage('Env Check') {
      steps {
        sh 'echo JAVA_HOME=$JAVA_HOME && java -version || true'
        sh 'node -v && npm -v || true'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Tests & Coverage') {
      steps {
        // adapt these to your package.json scripts if needed
        sh 'npm test || true'
        sh 'npm run coverage || true'
      }
    }

    stage('SonarCloud Analysis (wait for Quality Gate)') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SC_TOKEN')]) {
          sh '''
            set -e
            rm -rf sonar-scanner-*
            curl -sSL -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-macosx.zip
            unzip -q sonar-scanner.zip
            ./sonar-scanner-*/bin/sonar-scanner \
              -Dsonar.login=$SC_TOKEN \
              -Dsonar.qualitygate.wait=true
          '''
        }
      }
    }
  }
}
