def notify(String status) {
    withCredentials([
        string(credentialsId: 'webex-token', variable: 'WEBEX_TOKEN'),
        string(credentialsId: 'webex-room', variable: 'WEBEX_ROOM')
    ]) {
        sh """
            curl -s -X POST https://webexapis.com/v1/messages \\
              -H "Authorization: Bearer \$WEBEX_TOKEN" \\
              -H "Content-Type: application/json" \\
              -d '{"roomId": "'"\$WEBEX_ROOM"'", "text": "Jenkins build #${env.BUILD_NUMBER} of ${env.JOB_NAME}: ${status}"}'
        """
    }
}

pipeline {
    agent any
    triggers { githubPush() }
    stages {
        stage('Test') {
            steps { sh 'python3 -m unittest discover -v' }
        }
    }
    post {
        success { script { notify('SUCCESS') } }
        failure { script { notify('FAILURE') } }
    }
}
