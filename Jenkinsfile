pipeline {
    agent any

    environment {
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

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        sonar-scanner \
                          -Dsonar.organization=pramodyasenadheera \
                          -Dsonar.projectKey=pramodyasenadheera_8.2CDevSecOps \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
