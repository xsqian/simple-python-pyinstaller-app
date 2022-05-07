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
                    if (Math.random() > 0.01) {
                        throw new Exception()
                    }
                }
                writeFile file: 'test-results.txt', text: 'passed'
            }
        }
    }
}