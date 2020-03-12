pipeline {
    agent {
        any
    }
    stages {

        stage('Build Develop APK') {

            when {
                branch 'master'
            }
            steps {
               
                    sh './gradlew clean assembleRelease'
                
            }
            post {
                failure {
                    echo "Build Develop APK Failure!"
                }
                success {
                    echo "Build Develop APK Success!"
                }
            }
        }

        stage('Build Beta APK') {
            when {
                branch 'beta'
            }
            steps {
                
                    sh './gradlew clean assembleRelease'
                
            }
            post {
                failure {
                    echo "Build Beta APK Failure!"
                }
                success {
                    echo "Build Beta APK Success!"
                }
            }
        }

        stage('Build Prod APK') {
            when {
                branch 'prod'
            }
            steps {
               
                    sh './gradlew clean assembleRelease'
                
            }
            post {
                failure {
                    echo "Build Prod APK Failure!"
                }
                success {
                    echo "Build Prod APK success!"
                }
            }
        }
        stage('UnitTest'){
            steps{
              sh './gradle test'
            }
        
        }

        stage('Upload') {
            steps {
                archiveArtifacts(artifacts: 'app/build/outputs/apk/**/*.apk', fingerprint: true)
            }
            post {
                failure {
                    echo "Archive Failure!"
                }
                success {
                    echo "Archive Success!"
                }
            }
        }

        stage('Report') {
            steps {
                echo getChangeString()
            }
        }
    }
}

@NonCPS
def getChangeString() {
    MAX_MSG_LEN = 100
    def changeString = ""

    echo "Gathering SCM Changes..."
    def changeLogSets = currentBuild.changeSets
    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            truncated_msg = entry.msg.take(MAX_MSG_LEN)
            changeString += "[${entry.author}] ${truncated_msg}\n"
        }
    }

    if (!changeString) {
        changeString = " - No Changes -"
    }
    return changeString
}
