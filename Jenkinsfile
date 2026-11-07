
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Python') {
            steps {
                sh 'python3 --version'
            }
        }

        stage('Run Python Unit Tests') {
            steps {
                sh 'python3 -m unittest discover -v'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESSFUL - All Python tests passed!'
        }

        failure {
            echo 'BUILD FAILED - Check the console output.'
        }

        always {
            script {
                env.WEBEX_BUILD_STATUS = currentBuild.currentResult
            }

            withCredentials([
                string(
                    credentialsId: 'webex-bot-token',
                    variable: 'WEBEX_TOKEN'
                ),
                string(
                    credentialsId: 'webex-room-id',
                    variable: 'WEBEX_ROOM_ID'
                )
            ]) {
                sh '''
                    python3 - <<'PY'
import json
import os
import urllib.request

status = os.environ["WEBEX_BUILD_STATUS"]
job = os.environ.get("JOB_NAME", "Jenkins Pipeline")
build = os.environ.get("BUILD_NUMBER", "?")

message = (
    f"Jenkins Build Notification\\n"
    f"Pipeline: {job}\\n"
    f"Build: #{build}\\n"
    f"Status: {status}\\n"
)

if status == "SUCCESS":
    message += "Python unit tests completed successfully!"
else:
    message += "Build failed. Check Jenkins console output."

data = json.dumps({
    "roomId": os.environ["WEBEX_ROOM_ID"],
    "text": message
}).encode("utf-8")

request = urllib.request.Request(
    "https://webexapis.com/v1/messages",
    data=data,
    headers={
        "Authorization": "Bearer " + os.environ["WEBEX_TOKEN"],
        "Content-Type": "application/json"
    },
    method="POST"
)

with urllib.request.urlopen(request, timeout=20) as response:
    print("Webex notification sent. HTTP:", response.status)
PY
                '''
            }
        }
    }
}
