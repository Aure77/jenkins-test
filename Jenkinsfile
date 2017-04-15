#!groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                mvn 'clean install'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: '**/target/*.xml'
                }
            }
        }
        stage("Release confirmation") {
            timeout(time: 5, unit: 'MINUTES') {
                input 'Release project ?'
            }
        }
        stage("Release") {
            //mvn 'release:prepare'
            //mvn 'release:perform'
            echo 'Releasing....'
        } 
    }
}

def mvn(String args) {
    withMaven(jdk: 'jdk1.8', maven: 'Maven3') {
        sh "mvn -B ${args}"
    }
}