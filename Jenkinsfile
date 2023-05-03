pipeline {
    agent any
    tools {
        maven 'V3.9.1'
    }
    stages {
        stage('Checkout')
        {
            steps {
                git branch: 'main', credentialsId: '08239c21-315b-4576-b6ab-caa2d7325a02', url: 'git@github.com:aqw42/hello_world'
            }
        }
        stage('Generate')
        {
            steps {
                sh 'mvn generate-test-resources process-test-resources'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true -DtestFailureIgnore=true test'
            }
        }
        stage('Generate Jar') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true -DtestFailureIgnore=true package'
            }
        }
        stage('Process build') {
               parallel {
                    stage('Artifact Jar') {
                        steps {
                            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                        }
                    }
                    stage('Process Test reports') {
                        steps {
                            junit checksName: 'Jenkins Junit Tests', skipMarkingBuildUnstable: true, testResults: 'target/surefire-reports/*.xml'
                        }
                    }
               }
        }
    }
}
