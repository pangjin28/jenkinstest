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
    stage('Create index.html') {
  steps {
    powershell '''
      @"
<!doctype html>
<html>
<head><meta charset="utf-8"><title>Jenkins Test</title></head>
<body>
  <h1>Hello from Jenkins!</h1>
  <p>Build: $env:BUILD_NUMBER</p>
  <p>Time: $(Get-Date)</p>
</body>
</html>
"@ | Out-File -Encoding utf8 index.html
    '''
  }
}

stage('Start web server') {
  steps {
    // index.html 있는 현재 폴더를 8081로 서빙 (10초만 유지)
    bat '''
      start "" /B cmd /c "python -m http.server 8081"
      timeout /t 10 >nul
      for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8081 ^| findstr LISTENING') do taskkill /PID %%a /F >nul 2>&1
    '''
  }
}
  }
}
