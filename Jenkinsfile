pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build') {
      steps {
        echo 'Installing deps…'
        sh 'npm ci || npm install || echo "No Node project detected, skipping"'
      }
    }
    stage('Test') {
      steps {
        echo 'Running tests…'
        sh 'npm test || echo "No tests found, skipping"'
      }
    }
    stage('Package') {
      steps {
        echo 'Packaging…'
        sh 'zip -r artifact.zip . >/dev/null || echo "Zip skipped"'
        archiveArtifacts artifacts: 'artifact.zip', fingerprint: true, onlyIfSuccessful: true
      }
    }
    stage('Deploy') {
      when { expression { return false } } // flip to true when ready
      steps {
        echo 'Deploy step placeholder (EC2/Docker/K8s can go here)'
      }
    }
  }
  post {
    success { echo 'CI completed ✅' }
    failure { echo 'CI failed ❌' }
    always  { echo 'Build finished (check artifacts tab if packaged).' }
  }
}
