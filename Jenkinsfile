pipeline {
    agent any

    environment {
        TELEGRAM_TOKEN = credentials('telegram-token')
        DISCORD_WEBHOOK = credentials('discord-webhook')

        TELEGRAM_CHAT = '5062203012'
    }

    stages {

        stage('Clonar repositorio') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/thiagxrg/jenkis-real-proyecto.git'
            }
        }

        stage('Verificar Node') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Instalar dependencias Backend') {
            steps {
                dir('src/backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Instalar dependencias Frontend') {
            steps {
                dir('src/frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('src/frontend') {
                    sh 'npm run build'
                }
            }
        }
    }

    post {

        success {

            sh """
            curl -H "Content-Type: application/json" \
            -X POST \
            -d '{"content":"✅ Pipeline SUCCESS"}' \
            $DISCORD_WEBHOOK
            """

            sh """
            curl -s -X POST https://api.telegram.org/bot$TELEGRAM_TOKEN/sendMessage \
            -d chat_id=$TELEGRAM_CHAT \
            -d text="✅ Pipeline SUCCESS"
            """
             emailext (
            to: "thiagorojascole99@gmail.com",
            subject: "✅ SUCCESS Jenkins",
            body: "Pipeline terminado correctamente 🚀"
        )
        }

        failure {

            sh """
            curl -H "Content-Type: application/json" \
            -X POST \
            -d '{"content":"❌ Pipeline FAILED"}' \
            $DISCORD_WEBHOOK
            """

            sh """
            curl -s -X POST https://api.telegram.org/bot$TELEGRAM_TOKEN/sendMessage \
            -d chat_id=$TELEGRAM_CHAT \
            -d text="❌ Pipeline FAILED"
            """
        }

        unstable {

            sh """
            curl -H "Content-Type: application/json" \
            -X POST \
            -d '{"content":"⚠️ Pipeline UNSTABLE"}' \
            $DISCORD_WEBHOOK
            """

            sh """
            curl -s -X POST https://api.telegram.org/bot$TELEGRAM_TOKEN/sendMessage \
            -d chat_id=$TELEGRAM_CHAT \
            -d text="⚠️ Pipeline UNSTABLE"
            """
        }
    }
}