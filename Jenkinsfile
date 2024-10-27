pipeline {
    agent any
    stages {
        stage('Get Commit Info...') {
            steps {
                echo "Getting Commit Info"
                script {
                    try {
                        // Execute Git commands directly and capture the output
                        def authorName = bat(script: 'git log -1 --pretty=format:%%an', returnStdout: true).trim()
                        def authorEmail = bat(script: 'git log -1 --pretty=format:%%ae', returnStdout: true).trim()
                        
                        // Split each result by newline and get only the last line, which is the actual output
                        authorName = authorName.split("\r?\n")[-1].trim()
                        authorEmail = authorEmail.split("\r?\n")[-1].trim()

                        // Save committer info as environment variables
                        env.AUTHOR_NAME = authorName
                        env.AUTHOR_EMAIL = authorEmail
                        
                        env.AUTHOR_NAME_ENCODED = URLEncoder.encode(env.AUTHOR_NAME, "UTF-8").replace("%", "%%")
                        env.AUTHOR_EMAIL_ENCODED = URLEncoder.encode(env.AUTHOR_EMAIL, "UTF-8").replace("%", "%%")

                        println("(Author Name) value = ${authorName}")
                        println("(Author Email) value = ${authorEmail}")
                        println("(Author Name Encoded) value = ${env.AUTHOR_NAME_ENCODED}")
                        println("(Author Email Encoded) value = ${env.AUTHOR_EMAIL_ENCODED}")

                        echo "Commit Info Collected"
                    } catch (Exception e) {
                        echo "Error occurred while collecting GIT commit info"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        stage('Build') {
            steps {
                echo "Running build..."
                script {
                    try {
                        // echo 'Running actual commands here...'
                        bat "python app.py"
                        // bat "exit 0"  // Replace 'exit 0' with actual build commands
                    // throw new Exception("Custom Error")
                    } catch (Exception e) {
                            echo "Error occurred while executing build commands"
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
            
            echo "Build successful. Calling switch bulb API"
            bat """
                curl --location "http://localhost:8001/switch-bulb?author_name=${env.AUTHOR_NAME_ENCODED}&author_email=${env.AUTHOR_EMAIL_ENCODED}&build_status=true"
            """
        }
        failure {
            // API call for failure with committer info
            echo "Build failed. Calling switch bulb API"
            bat """
                curl --location "http://localhost:8001/switch-bulb?author_name=${env.AUTHOR_NAME_ENCODED}&author_email=${env.AUTHOR_EMAIL_ENCODED}&build_status=false"
            """
        }
    }
}
