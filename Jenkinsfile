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
                jsAnalysis: [   // all parameters specific to Javascript analysis go under this
                    enable: true,
                    force: true,                            // if set, it forces analysis even there are no vulnerable component method signatures (internal use only)
                    //node: [                                 // optional
                    //  executable: '/path/to/node/exec'      // absolute path expected; you can use env. vars. e.g. "${env.WORKSPACE}/path/to/node"
                    //],
                    projectDirectory: '.',                  // relative to workspace; optional; default: <workspace-dir>
                    packageJsonFile: 'package.json',        // relative to workspace; optional; default: <workspace-dir>/package.json
                    sourceFiles: [                          // DO NOT include anything from node_modules here!
                        [pattern: 'app/**/*.js']            // relative to workspace; Ant-style glob patterns are supported; DO NOT include file extensions in the patterns
                    ],
                    excludeFiles: [                         // relative to workspace; optional; Ant-style glob patterns are supported
                        [pattern: 'test/**/*']
                    ]
                ]
            ]
        )
      }
    }
  }
}
