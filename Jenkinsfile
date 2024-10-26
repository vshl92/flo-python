pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Hello World From Jenkins File"
                script {
                    try {
                        // Get the latest committer's details
                        def committerName = bat(script: "git log -1 --pretty=format:\"%an\"", returnStdout: true).trim()
                        def committerEmail = bat(script: "git log -1 --pretty=format:\"%ae\"", returnStdout: true).trim()
                        // Save committer info as environment variables
                        env.COMMITTER_NAME = committerName
                        env.COMMITTER_EMAIL = committerEmail

                        // Place your build commands here
                        echo 'Running actual commands here...'
                        echo 'Running build...'
                        // sh 'exit 0'  // Replace 'exit 0' with actual build commands
                        // throw new Exception("Custom Error")

                    } catch (Exception e) {
                        // currentBuild.result = 'FAILURE'
                        // throw e
                        echo 'Error Occurred'
                        throw e
                    }
                }
            }
        }
    }
    post {
        success {
            // API call for success with committer info
            echo "Post Build Status - Success"
            bat """
                curl --location "http://localhost:8001/switch-bulb?committer_id=1234&commit_status=true"
            """
        }
        failure {
            // API call for failure with committer info
            echo "Post Build Status - Failed"
            bat """
                curl --location "http://localhost:8001/switch-bulb?committer_id=1234&commit_status=false"
            """
        }
    }
}
