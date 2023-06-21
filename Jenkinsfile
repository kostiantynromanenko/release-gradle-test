pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    stages {
        stage('test') {
            steps {
                echo 'Starting to run tests'
                sh "python -m pytest"
            }
        }
    }
}
