pipeline {
    agent any

    stages {
        stage('Run hello.sh') {
            steps {
                sh 'chmod +x hello.sh'
                sh './hello.sh'
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
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; background-color: #f4f6f8; margin: 0; padding: 24px; }
        .card { max-width: 600px; margin: 0 auto; background: #ffffff; border-radius: 8px; border: 1px solid #e0e0e0; border-top: 5px solid #d32f2f; padding: 28px; box-shadow: 0 4px 12px rgba(0,0,0,0.08); }
        h2 { color: #d32f2f; margin-top: 0; font-size: 22px; }
        p { color: #424242; font-size: 15px; line-height: 1.6; }
        .info-table { width: 100%; border-collapse: collapse; margin: 16px 0; }
        .info-table td { padding: 8px 12px; border-bottom: 1px solid #eeeeee; font-size: 14px; }
        .info-table td.label { font-weight: bold; color: #616161; width: 30%; }
        .info-table td.value { color: #212121; }
        .console-box { background-color: #263238; color: #eceff1; padding: 14px; border-radius: 6px; font-family: monospace; font-size: 13px; word-break: break-all; margin: 16px 0; }
        .console-box a { color: #80cbc4; text-decoration: none; }
        .btn { display: inline-block; padding: 12px 24px; background-color: #d32f2f; color: #ffffff !important; text-decoration: none; border-radius: 6px; font-weight: bold; font-size: 14px; margin-top: 8px; }
    </style>
</head>
<body>
    <div class="card">
        <h2>Build Failed: ${env.JOB_NAME}</h2>
        <p>Job <b>${env.JOB_NAME}</b> build <b>#${env.BUILD_NUMBER}</b> encountered an error while executing <code>hello.sh</code>.</p>
        
        <table class="info-table">
            <tr>
                <td class="label">Job Name:</td>
                <td class="value">${env.JOB_NAME}</td>
            </tr>
            <tr>
                <td class="label">Build Number:</td>
                <td class="value">#${env.BUILD_NUMBER}</td>
            </tr>
            <tr>
                <td class="label">Status:</td>
                <td class="value" style="color: #d32f2f; font-weight: bold;">FAILURE</td>
            </tr>
        </table>

        <p>You can view the full console output log below:</p>
        <div class="console-box">
            <a href="${consoleUrl}">${consoleUrl}</a>
        </div>

        <a href="${consoleUrl}" class="btn">View Console Output</a>
    </div>
</body>
</html>
"""
            }

            emailext (
                to: '<your-email>',
                subject: "FAILED: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                body: readFile('console_report.html'),
                mimeType: 'text/html',
                attachmentsPattern: 'console_report.html'
            )
        }
    }
}
