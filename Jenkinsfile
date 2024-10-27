pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Hello World From Jenkins File 1"
                script {
                    try {
        
                        
                        // // Get the latest committer's details
                        // def committerName = bat(script: "git log -1 --pretty=format:\"%an\"", returnStdout: true).trim()
                        // def committerEmail = bat(script: "git log -1 --pretty=format:\"%ae\"", returnStdout: true).trim()
                        // // Save committer info as environment variables
                        // env.COMMITTER_NAME = committerName
                        // env.COMMITTER_EMAIL = committerEmail

                        // Execute Git commands directly and capture the output
                        def committerName = bat(script: 'git log -1 --pretty=format:%an', returnStdout: true).trim()
                        def committerEmail = bat(script: 'git log -1 --pretty=format:%ae', returnStdout: true).trim()

                        
                        echo "============================="
                        bat "git log -1"
                        // bat "git log -1 --pretty=format:%an"
                        // bat "git log -1 --pretty=format:%ae"
                        def temp = bat(script: 'git log -1', returnStdout: true).trim()
                        env.TEMP = temp
                        echo "TEMP .. ${env.TEMP}"
                        echo "============================="
                        
                        
                        
                        // Save committer info as environment variables
                        env.COMMITTER_NAME = committerName
                        env.COMMITTER_EMAIL = committerEmail

                        // Create temporary .bat file to capture committer name
                        // writeFile file: 'getCommitterName.bat', text: '@echo off\n' +
                        //     '"C:\\Program Files\\Git\\bin\\git.exe" log -1 --pretty=format:"%an" > committerName.txt'
                        // bat 'getCommitterName.bat'
                        // def committerName = readFile('committerName.txt').trim()
                        
                        // // Create temporary .bat file to capture committer email
                        // writeFile file: 'getCommitterEmail.bat', text: '@echo off\n' +
                        //     '"C:\\Program Files\\Git\\bin\\git.exe" log -1 --pretty=format:"%ae" > committerEmail.txt'
                        // bat 'getCommitterEmail.bat'
                        // def committerEmail = readFile('committerEmail.txt').trim()
                        
                        // // Save committer info as environment variables
                        // env.COMMITTER_NAME = committerName
                        // env.COMMITTER_EMAIL = committerEmail

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
            echo "Build successful, calling API with success=true, committer=${env.COMMITTER_NAME} (${env.COMMITTER_EMAIL})"
            bat """
                curl --location "http://localhost:8001/switch-bulb?committer_id=1234&commit_status=true"
            """
        }
        failure {
            // API call for failure with committer info
            echo "Build failed, calling API with success=false, committer=${env.COMMITTER_NAME} (${env.COMMITTER_EMAIL})"
            bat """
                curl --location "http://localhost:8001/switch-bulb?committer_id=1234&commit_status=false"
            """
        }
    }
}
