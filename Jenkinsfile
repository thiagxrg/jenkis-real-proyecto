pipeline {
    agent any

    environment {
            TELEGRAM_TOKEN = '8924366882:AAHU9geNnx4qwWyM8fWiVS4dk3XxyD3pWtk'
        TELEGRAM_CHAT = '5062203012'
        DISCORD_WEBHOOK = 'https://discord.com/api/webhooks/1506381725335421141/6vZ0c_7Xmex2SoNvLPcvgrVg2-4gW8paUdi3AEE6XZNNHWq_xc4C_80B-d5SuMEpGmPP'
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