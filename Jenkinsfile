#!/usr/bin/env groovy

def setJobProperties() {
  properties([
    [
      $class: 'BuildDiscarderProperty',
      strategy: [
        $class: 'BuildRotator',
        daysToKeep: 5,
        numToKeep: 10,
        artifactsDaysToKeep: 5,
        artifactsNumToKeep: 10
      ]
    ]
  ])
}

pipeline {
    agent {
        node {
            label 'master'
        }
    }
    stages {
        stage ('Code style linting') {
            steps {
                script {
                    ansiColor('xterm') {
                        println "\u001B[34m\u001B[1m******************\u001B[0m"
                        println "\u001B[34m\u001B[1mCode Style Linting\u001B[0m"
                        println "\u001B[34m\u001B[1m******************\u001B[0m"
                        try {
                            sh '''
                                #!/bin/bash
                                set -e
                                python3 -m pycodestyle --ignore=E501 */*.py
                            '''
                            println "\u001B[32mCode adheres to PyCodeStyle\u001B[0m"
                        } catch (Exception e) {
                            println "\u001B[31m********************************************************\u001B[0m"
                            println "\u001B[31mFAILED: Code style linting failed. Please fix the issues\u001B[0m"
                            println "\u001B[31m********************************************************\u001B[0m"
                            sh 'exit 1'
                        }
                    }   
                }
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
