#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
//                     mvn 'clean install'
                script {
                    def pom = readMavenPom file: 'pom.xml'
                    def version = pom.version.replace("-SNAPSHOT", "")
                    def versionArr = version.split('\\.')
                    versionArr[-1] += 1 // increment last digit
                    env.nextMavenReleaseVersion = versionArr.join('.')
                }
            }
        }
        stage("Release confirmation") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        def releaseVersion = input(
                            id: 'releaseVersion', message: 'Release project ?', parameters: [
                                [$class: 'TextParameterDefinition', defaultValue: "${env.nextMavenReleaseVersion}", description: 'release version', name: 'releaseVersion']
                            ]
                        )
                    }
                }
            }
        }
        stage("Release") {
            steps {
                echo "release:prepare -DreleaseVersion=${env.nextMavenReleaseVersion}"
               // mvn "release:prepare -DreleaseVersion=${env.nextMavenReleaseVersion}"
               // mvn "release:perform -DreleaseVersion=${env.nextMavenReleaseVersion}"
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
