
pipeline {
    agent any

    stages {
        stage('Run hello.sh') {
            steps {
                sh 'chmod +x hello.sh'
                sh './hello.sh'

                // Force the build to fail for email testing
                sh 'exit 1'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            script {
                def consoleUrl = "${env.BUILD_URL}console"

                writeFile file: 'console_report.html', text: """
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Build Failed</title>
</head>
<body style="font-family: Arial, sans-serif; background:#f4f6f8; padding:24px;">
    <div style="max-width:600px; margin:auto; background:white; padding:28px; border-radius:8px; border-top:5px solid #d32f2f;">
        <h2 style="color:#d32f2f;">Build Failed: ${env.JOB_NAME}</h2>

        <p>Jenkins build #${env.BUILD_NUMBER} has failed.</p>

        <p><b>Job Name:</b> ${env.JOB_NAME}</p>
        <p><b>Build Number:</b> #${env.BUILD_NUMBER}</p>
        <p><b>Status:</b> <span style="color:#d32f2f;">FAILURE</span></p>

        <p>Click below to view the full console output:</p>

        <a href="${consoleUrl}"
           style="display:inline-block; padding:12px 24px; background:#d32f2f; color:white; text-decoration:none; border-radius:6px;">
           View Console Output
        </a>
    </div>
</body>
</html>
"""
            }

            emailext(
                to: 'ruwankhaled657@gmail.com',
                subject: "FAILED: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                body: readFile('console_report.html'),
                mimeType: 'text/html'
            )
        }
    }
}
