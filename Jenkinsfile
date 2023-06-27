pipeline {
    agent any

    options {
        ansiColor("xterm")
    }

    parameters {
        booleanParam(name: "RELEASE", defaultValue: false, description: "Release?")
    }

    stages {
       stage("Build") {
            steps {
                echo "Build...."
            }
       }
       stage("Update version") {
           steps {
                script {
                       withCredentials([gitUsernamePassword(credentialsId: "70ef45f2-c933-442a-9364-71271ffc86d8")]) {
                           sh "./gradlew incrementVersion --versionIncrementType=PATCH -Psnapshot=false"
                           sh "git add build.gradle"
                           sh "git commit -m \"[Bump version] New version: ${version}\""
                           sh "git push"
                       }
                  }
           }
       }
       stage("Release") {
            when {
                allOf {
                   expression { env.BRANCH_NAME == "master" }
                   expression { params.RELEASE == true }
                }
            }
            steps {
                script {
                    def RELEASE_VERSION = sh(
                        script: "./gradlew -q getReleaseVersion",
                        returnStdout: true
                    )
                    withCredentials([gitUsernamePassword(credentialsId: "70ef45f2-c933-442a-9364-71271ffc86d8")]) {
                      sh ("""
                        git checkout -b release/${RELEASE_VERSION}
                        git tag ${version}
                        git push origin --tags
                        git checkout tags/${version}
                      """)
                    }
                    buildPlugin {
                        args = ['gradle': [
                          'tasks': ['publish'],
                          'credentialId': '1e931704-4639-4d11-b40a-4fbed9e87680'
                        ]]
                    }
                }
            }
       }
    }
}
