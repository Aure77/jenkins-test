#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
//                     mvn 'clean install'
                script {
                    def version = readMavenPom;
                    echo 'InScript=$version'
                }
                echo 'Project=$version'
            }
        }
        stage("Release confirmation") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        def releaseVersion = input(
                            id: 'releaseVersion', message: 'Release project ?', parameters: [
                                [$class: 'TextParameterDefinition', defaultValue: '1.0.0', description: 'release version', name: 'releaseVersion']
                            ]
                        )
                    }
                }
            }
        }
        stage("Release") {
            steps {
                echo 'release:prepare -DreleaseVersion=$releaseVersion'
                println 'release:perform -DreleaseVersion=${env.releaseVersion}'
               // mvn 'release:prepare'
               // mvn 'release:perform'
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