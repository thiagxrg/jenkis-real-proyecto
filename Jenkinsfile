pipeline {
    agent any

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
            echo 'Pipeline SUCCESS'
        }

        failure {
            echo 'Pipeline FAILED'
        }

        unstable {
            echo 'Pipeline UNSTABLE'
        }
    }
}