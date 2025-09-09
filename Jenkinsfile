pipeline {
  agent any

  environment {
    // Java 17 from Homebrew on Apple Silicon
    JAVA_HOME = "/opt/homebrew/opt/openjdk@17"
    PATH = "${JAVA_HOME}/bin:/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git'
      }
    }

    stage('Env Check') {
      steps {
        sh '''
          set -e
          echo "JAVA_HOME=$JAVA_HOME"
          java -version
          node -v
          npm -v
        '''
      }
    }

    stage('Install') {
      steps {
        sh 'npm ci || npm install'
      }
    }

    // Optional: quick credential sanity check
    stage('Creds test') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh 'echo "SONAR_TOKEN length: $(printf %s "$SONAR_TOKEN" | wc -c)"'  // value will be masked
        }
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            set -eux
            rm -rf sonar-scanner-* sonar-scanner.zip
            curl -sSL -o sonar-scanner.zip \
              https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-macosx.zip
            unzip -q sonar-scanner.zip
            ./sonar-scanner-*/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN
          '''
        }
      }
    }
  }
}
