pipeline {
    agent any

    options {
        ansiColor("xterm")
    }

    parameters {
        booleanParam(name: "RELEASE", defaultValue: false, description: "Release?")
    }

    stages {
        stage('Checkout') {
           when {
               allOf {
                  expression { params.RELEASE == false }
               }
           }
            steps {
                scmSkip(deleteBuild: true, skipPattern:'.*\\[ci skip\\].*')
            }
        }
       stage("Build") {
            steps {
                echo "Build...."
            }
       }
       stage("Update version") {
           when {
               allOf {
                  expression { params.RELEASE == false }
               }
           }
           steps {
                script {
                       withCredentials([gitUsernamePassword(credentialsId: "70ef45f2-c933-442a-9364-71271ffc86d8")]) {
                           sh "./gradlew incrementVersion --versionIncrementType=PATCH -Psnapshot=false"
                           sh "git add build.gradle"
                            def VERSION = sh(
                               script: "./gradlew -q getVersion",
                               returnStdout: true
                            )
                           sh "git commit -m \"[ci skip] New version: ${VERSION}\""
                           sh "git push origin HEAD:master --force"
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
                     def VERSION = sh(
                        script: "./gradlew -q getVersion",
                        returnStdout: true
                     )
                    withCredentials([gitUsernamePassword(credentialsId: "70ef45f2-c933-442a-9364-71271ffc86d8")]) {
                      sh ("""
                        git checkout -b release/${RELEASE_VERSION}
                        git tag ${VERSION}
                        git push origin --tags
                        git checkout tags/${VERSION} -b ${VERSION}
                        ./gradlew publish
                      """)
                    }
                }
            }
       }
    }
    post {
        always {
            cleanWs()
        }
    }
}
