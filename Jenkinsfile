pipeline {
  agent any

  tools {
    nodejs 'node 16'
  }

  options {
    buildDiscarder logRotator(
        artifactDaysToKeepStr: '',
        artifactNumToKeepStr: '',
        daysToKeepStr: '1',
        numToKeepStr: '5'
    )
  }

  environment {
    CI = 'true'
  }

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Policy Evaluation') {
      steps {
        nexusPolicyEvaluation(
            enableDebugLogging: false,
            iqStage: env.GIT_BRANCH == 'master' ? 'build': 'develop',
            iqApplication: 'NodeGoat',
            failBuildOnNetworkError: true,
            iqScanPatterns: [
                [scanPattern: 'package.json'],
                [scanPattern: 'package-lock.json']
            ],
            reachability: [
                logLevel: 'DEBUG',
                jsAnalysis: [
                    enable: true,
                    force: true,
                    sourceFiles: [
                        [pattern: 'app/**']
                    ],
                    excludeFiles: [
                        [pattern: 'test/**']
                    ]
                ]
            ]
        )
      }
    }
  }
}
