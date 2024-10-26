pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        echo "Hello World From Jenkins File"
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }
    post {
        success {
            // API call for success with committer info
            echo "Build successful"
            sh """
                curl --location 'http://localhost:8001/switch-bulb?committer_id=1234&commit_status=true'
            """
        }
        failure {
            // API call for failure with committer info
            echo "Build failed"
            sh """
                curl --location 'http://localhost:8001/switch-bulb?committer_id=1234&commit_status=false'
            """
        }
    }
}
