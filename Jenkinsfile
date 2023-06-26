pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Release?')
    }

    stages {
       when {
            allOf {
                expression { env.BRANCH_NAME == 'master' }
                expression { params.RELEASE == true }
            }
       }
       stage('Build') {
            steps {
                echo 'Build....'
            }
       }
    }
}
