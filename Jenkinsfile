pipeline {
    agent any

    options {
        ansiColor("xterm")
    }

    parameters {
        booleanParam(name: "RELEASE", defaultValue: false, description: "Release?")
    }

    stages {
       stage("Release") {
            when {
                allOf {
                   expression { env.BRANCH_NAME == "master" }
                   expression { params.RELEASE == true }
                }
            }
            steps {
                script {
                    withCredentials([gitUsernamePassword(credentialsId: "70ef45f2-c933-442a-9364-71271ffc86d8")]) {
                      def RELEASE_VERSION = sh(
                         script: "./gradlew -q getReleaseVersion",
                         returnStdout: true
                      ).trim()
                      sh ("""
                        git checkout -b release/${RELEASE_VERSION}
                        ./gradlew unSnapshotVersion
                      """)
                    }
                }
            }
       }
    }
}
