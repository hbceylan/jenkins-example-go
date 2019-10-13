node {
    try {
        currentBuild.result = "SUCCESS"
        commitId = ""

        stage("Preparation") {
          env.TEST_MESSAGE="hello!"
        }

        stage("Cleanup Workspace") {
          deleteDir()
        }

        stage("Download Code") {
            checkout scm
            commitId = sh(returnStdout: true, script: 'git rev-parse HEAD')
        }

        if (params.env!="prod" && params.env!="staging") {
          stage('Build') {
                sh "echo ${TEST_MESSAGE}"
            }
          stage('Build') {
                sh "go run main.go"
            }
        }


        stage('Deploy') {
            withCredentials([
                string(credentialsId: "TEST_PASS", variable: 'TEST_PASS'),
                string(credentialsId: "TEST_CRED", variable: 'TEST_CRED')]) {
               dir('.') {
                sh "echo ${params.env} $TEST_PASS $TEST_CRED"
               }
            }
          }

        currentBuild.result = "SUCCESS"

    } catch (e) {
      currentBuild.result = "FAILED"
      throw e
    } finally {
    }
 }