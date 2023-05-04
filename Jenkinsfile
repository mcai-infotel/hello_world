pipeline {
    agent any
    tools {
        maven 'V3.9.1'
    }
    stages {
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
    post {
        always{
            echo "Finished"
        }
        
        fixed {
            emailext attachLog: true, body: '', subject: 'Hello world is fixed, well done !', to: 'corentin.noel56@gmail.com'
        }
    
        failure {
            emailext attachLog: true, body: '', subject: 'Hello world is broken, boohoo !', to: 'corentin.noel56@gmail.com'
        }
    }
}
