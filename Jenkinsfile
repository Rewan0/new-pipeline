```groovy
pipeline {
    agent any

    stages {
        stage('Hello World') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Run Script') {
            steps {
                sh '''
                    chmod +x hello.sh
                    ./hello.sh
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success, failure, or aborted).'
        }

        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed! Sending failure email...'

            emailext(
                to: 'ruwankhaled657@gmail.com',
                subject: "BUILD FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                mimeType: 'text/plain',
                body: """
Hello,

Your Jenkins Pipeline has FAILED!

Job Name: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Please check the attached Console Log to identify
the reason for the failure.

Regards,
Jenkins
""",
                attachLog: true
            )
        }

        aborted {
            echo 'Pipeline was aborted!'
        }
    }
}
```
