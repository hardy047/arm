pipeline {
  agent {
    dockerfile {
      filename 'test'
    }

  }
  stages {
    stage('clone') {
      steps {
        git(url: 'https://github.com/hardy047/arm.git', branch: 'master')
      }
    }

  }
}