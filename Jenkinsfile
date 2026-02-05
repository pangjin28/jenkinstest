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
    powershell '''
      [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
      Get-ChildItem | Format-Table -AutoSize
    '''
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
