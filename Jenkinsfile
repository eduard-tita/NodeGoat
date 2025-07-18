pipeline {
  agent {
    docker {
      image 'node:lts-buster-slim'
      args '-p 3000:3000'
    }
  }
  environment {
    CI = 'true'
  }
  stages {
    stage('Build') {
      steps {
        sh 'npm -v'
        //sh 'npm install'
      }
    }
    //stage('Test') {
    //  steps {
    //    sh 'npm test'
    //  }
    //}
    stage('Policy Evaluation') {
      steps {
        nexusPolicyEvaluation iqApplication: 'NodeGoat', iqStage: env.GIT_BRANCH == 'master' ? 'build': 'develop',
            iqScanPatterns: [
                [scanPattern: 'package.json'], [scanPattern: 'package-lock.json']
            ]
      }
    }
  }
}
