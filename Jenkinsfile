pipeline {
  agent any

  environment {
    JAVA_HOME = "/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home"
    PATH = "$JAVA_HOME/bin:/opt/homebrew/bin:/usr/local/bin:$PATH"
    SONAR_TOKEN = credentials('3ae1e8e383c0a894c8793c6b2b4f7204f050bec7')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git'
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

    stage('SonarCloud Analysis') {
      steps {
        sh '''
          set -e
          rm -rf sonar-scanner-* sonar-scanner.zip
          curl -sSL -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-macosx.zip
          unzip -oq sonar-scanner.zip
          ./sonar-scanner-*/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN
        '''
      }
    }
  }
}
