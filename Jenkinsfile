pipeline {
    agent any

    tools {
        nodejs "node"
    }

    environment {
        DISCORD_WEB_HOOK_URL = credentials('DISCORD_WEB_HOOK_URL')
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    options {
        timeout(time: 90, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr:'3'))
    }

    stages {
        stage('Build') {
            steps {
                echo 'Deploying new version of the docs...'
                sh 'firebase deploy --token $FIREBASE_TOKEN'
            }
        }
    }

    post {
        always {
            discordSend description: "Documentation deployment", footer: "Nineva Docs", link: 'https://docs.ninevastudios.com', result: currentBuild.currentResult, title: JOB_NAME, webhookURL: env.DISCORD_WEB_HOOK_URL
        }
    }
}