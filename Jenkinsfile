pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Release?')
    }

    stages {
       stage('Release') {
            when {
                allOf {
                   expression { env.BRANCH_NAME == 'master' }
                   expression { params.RELEASE == true }
                }
            }
            steps {
                echo 'Build....'
            }
       }
    }
}
