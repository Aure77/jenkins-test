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
        def userInput = null;
        stage("Release confirmation") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        userInput = input(
                            id: 'userInput', message: 'Release project ?', parameters: [
                                [$class: 'TextParameterDefinition', defaultValue: '1.0.0', description: 'release version', name: 'releaseVersion']
                            ]
                        )
                    }
                    echo ("Env: "+userInput)
                }
            }
        }
        stage("Release") {
            steps {
                echo 'release='
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