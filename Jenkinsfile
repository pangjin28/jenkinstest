pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Show files') {
      steps {
        bat 'chcp 65001 >nul && dir'
      }
    }

    stage('Show content') {
      steps {
        bat '''
          if exist test.txt (
            echo ==== test.txt ====
            type test.txt
          ) else (
            echo test.txt not found
          )
        '''
      }
    }
  }
}
