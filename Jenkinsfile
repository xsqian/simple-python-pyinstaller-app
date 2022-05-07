pipeline {
    agent any
    environment {
        RELEASE='20.04'
    } 
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:3.8' 
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        stage('Test') {
            steps {
                echo "Testing release ${RELEASE}"
                script {
                    if (Math.random() > 0.99) {
                        throw new Exception()
                    }
                }
                writeFile file: 'test-results.txt', text: 'passed'
            }
        }
    }
    post {
        success {
            archiveArtifacts 'test-results.txt'
            slackSend channel: '#builds',
                        color: 'good',
                        message: "Release ${env.RELEASE}, success: ${currentBuild.fullDisplayName}."
        }
        failure {
            slackSend channel: '#builds',
                        color: 'danger',
                        message: "Release ${env.RELEASE}, FAILD: ${currentBuild.fullDisplayName}."
        }
    }
}