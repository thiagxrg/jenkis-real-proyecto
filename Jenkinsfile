pipeline {
    agent any

    environment {
        DISCORD_WEBHOOK = 'https://discord.com/api/webhooks/TU_WEBHOOK'
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
        }

        failure {
            sh """
            curl -H "Content-Type: application/json" \
            -X POST \
            -d '{"content":"❌ Pipeline FAILED"}' \
            $DISCORD_WEBHOOK
            """
        }

        unstable {
            sh """
            curl -H "Content-Type: application/json" \
            -X POST \
            -d '{"content":"⚠️ Pipeline UNSTABLE"}' \
            $DISCORD_WEBHOOK
            """
        }
    }
}