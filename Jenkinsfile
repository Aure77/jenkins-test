#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
//                     mvn 'clean install'
                script {
                    def pom = readMavenPom file: 'pom.xml'
                    env.currentBuildVersion = pom.version
                }
            }
        }
        stage("Release confirmation") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        def releaseVersion = input(
                            id: 'releaseVersion', message: 'Release project ?', parameters: [
                                [$class: 'TextParameterDefinition', defaultValue: "${env.currentBuildVersion}", description: 'release version', name: 'releaseVersion']
                            ]
                        )
                    }
                }
            }
        }
        stage("Release") {
            steps {
                echo "release:prepare -DreleaseVersion=${env.currentBuildVersion}"
               // mvn "release:prepare -DreleaseVersion=${env.currentBuildVersion}"
               // mvn "release:perform -DreleaseVersion=${env.currentBuildVersion}"
            }
        }
    }
     post {
         always {
             junit allowEmptyResults: true, testResults: '**/target/*.xml'
         }
     }
}

def mvn(String args) {
    withMaven(jdk: 'jdk1.8', maven: 'Maven3') {
        sh "mvn -B ${args}"
    }
}