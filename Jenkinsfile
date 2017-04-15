#!groovy
pipeline {
    agent any

    stages {
    //     stage('Build') {
    //         steps {
    //             echo 'Building..'
    //             mvn 'clean install'
    //         }
    //         post {
    //             always {
    //                 junit allowEmptyResults: true, testResults: '**/target/*.xml'
    //             }
    //         }
    //     }
        stage("Release confirmation") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        env.userInput = input(
                            id: 'userInput', message: 'Release project ?', parameters: [
                                [$class: 'TextParameterDefinition', defaultValue: '1.0.0', description: 'release version', name: 'releaseVersion']
                            ]
                        )
                    }
                }
            }
        }
        stage("Release") {
            steps {
                echo 'release=' + env.userInput
               // mvn 'release:prepare'
               // mvn 'release:perform'
            }
        } 
    }
}

def mvn(String args) {
    withMaven(jdk: 'jdk1.8', maven: 'Maven3') {
        sh "mvn -B ${args}"
    }
}