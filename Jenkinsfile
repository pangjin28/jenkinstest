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
    // 8081 포트로 index.html을 서빙 (10초만 실행하고 종료)
    powershell '''
      $port = 8081
      $listener = New-Object System.Net.HttpListener
      $listener.Prefixes.Add("http://localhost:$port/")
      $listener.Start()
      Write-Host "Serving http://localhost:$port/ (10 seconds)..."

      $end = (Get-Date).AddSeconds(10)
      while ((Get-Date) -lt $end) {
        if ($listener.IsListening -and $listener.Pending()) {
          $ctx = $listener.GetContext()
          $path = Join-Path (Get-Location) "index.html"
          $bytes = [System.IO.File]::ReadAllBytes($path)
          $ctx.Response.ContentType = "text/html; charset=utf-8"
          $ctx.Response.ContentLength64 = $bytes.Length
          $ctx.Response.OutputStream.Write($bytes, 0, $bytes.Length)
          $ctx.Response.OutputStream.Close()
        } else {
          Start-Sleep -Milliseconds 200
        }
      }

      $listener.Stop()
      $listener.Close()
      Write-Host "Server stopped."
    '''
  }
}
  }
}
