pipeline {
  agent any

  environment {
    JAVA_HOME = "/opt/homebrew/opt/openjdk@17"
    PATH = "$JAVA_HOME/bin:/opt/homebrew/bin:/usr/local/bin:$PATH"
    SONAR_TOKEN = credentials('eb76a58041d0835d570362fe2583ef3e68c8b262')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/pramodyasenadheera/8.2CDevSecOps.git'
      }
    }
    stage('Env Check') {
      steps { sh 'echo JAVA_HOME=$JAVA_HOME && java -version && node -v && npm -v' }
    }
    stage('Install') {
      steps { sh 'npm install' }
    }
  }
}
