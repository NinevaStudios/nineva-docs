pipeline {
    agent any

    environment {
        DISCORD_WEB_HOOK_URL = credentials('DISCORD_WEB_HOOK_URL')
    }

    options {
        timeout(time: 90, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr:'3'))
    }

    stages {
        stage('Build') {
            steps {

            }
        }

        stage('Update version') {
            steps {
                echo 'Updating version...'
                script {
                    def versionCode = calculateVersionCode(VERSION)
                    // TODO update versions in .uplugin file, commit, push and tag
                }
            }
        }
    }

    post {
        always {
            discordSend description: "Documentation deployment", footer: "Nineva Docs", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: env.DISCORD_WEB_HOOK_URL
        }
    }
}