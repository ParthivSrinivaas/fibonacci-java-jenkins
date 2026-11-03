
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh '''
                    set -eu
                    mkdir -p build/classes
                    javac -d build/classes Fibonacci.java
                '''
            }
        }

        stage('Package') {
            steps {
                sh '''
                    jar --create \
                        --file build/fibonacci.jar \
                        --main-class Fibonacci \
                        -C build/classes .
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    test -f build/fibonacci.jar
                    jar --list --file build/fibonacci.jar
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'build/fibonacci.jar',
                             fingerprint: true
            echo 'Build completed successfully!'
        }

        failure {
            echo 'Build failed. Check Console Output.'
        }
    }
}
