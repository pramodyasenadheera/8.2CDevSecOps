pipeline {
  agent any

  environment {
    // If you installed Temurin 17 via: brew install --cask temurin17
    JAVA_HOME = "/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home"

    // If instead you installed Homebrew openjdk@17, use this line and remove the Temurin one:
    // JAVA_HOME = "/opt/homebrew/opt/openjdk@17"

    PATH = "${JAVA_HOME}/bin:/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
    SONAR_TOKEN = credentials('b102ee90f5192b1bbdb45f453cc530260453a882')
  }

  stages {
    stage('Checkout')            { steps { git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git' } }
    stage('Install Dependencies'){ steps { sh 'npm install' } }
    stage('Run Tests')           { steps { sh 'npm test || true' } }
    stage('Generate Coverage')   { steps { sh 'npm run coverage || true' } }
    stage('NPM Audit (Security)'){ steps { sh 'npm audit || true' } }
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



