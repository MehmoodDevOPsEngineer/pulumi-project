pipeline {
  agent {
    kubernetes {
      label 'jenkins-agent'  // Must match the label from your Pod Template
    }
  }

  environment {
    AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    PULUMI_ACCESS_TOKEN = credentials('pulumi-access-token')
  }

  stages {
    stage('Checkout') {
      steps {
        container('python') {
          checkout scm
        }
      }
    }

    stage('Install Python Dependencies') {
      steps {
        container('python') {
          sh '''
            python -m pip install --upgrade pip
            pip install -r requirements.txt
          '''
        }
      }
    }

    stage('Pulumi Deploy') {
      steps {
        container('pulumi') {
          sh '''
            pulumi login
            pulumi stack select dev
            pulumi up --yes
          '''
        }
      }
    }
  }
}
