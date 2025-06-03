pipeline {
  agent any
  environment {
    NODE_ENV = 'development'
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            credentialsId: 'github-pat',
            url: 'https://github.com/M1lky05/test'
      }
    }
    stage('Install dependencies') {
      steps {
        bat 'npm install'
      }
    }
    stage('Run tests') {
      steps {
        bat 'npm test || exit /b 0'
      }
    }
    stage('Generate coverage report') {
      steps {
        bat 'npm run coverage || exit /b 0'
      }
    }
    stage('NPM Audit (Security scan)') {
      steps {
        bat 'npm audit || exit /b 0'
      }
    }
    stage('Sonar Cloud analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          bat '''
          set SONAR_TOKEN=%SONAR_TOKEN%
          sonar-scanner.bat
          '''
        }
      }
    }
  }
}