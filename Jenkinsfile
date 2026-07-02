pipeline {

    agent {
        label 'agent-windows'   // Tu peux garder ce label, même s'il pointe vers un Mac.
    }

    environment {
        DOCKERHUB_USER = "ditdevops1"
        BACKEND_IMAGE  = "${DOCKERHUB_USER}/backend-gestion-employe-l2dit"
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/frontend-gestion-employe-l2dit"

        BACKEND_IMAGE_TAG = "1.${BUILD_NUMBER}"
        FRONTEND_TAG      = "1.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Backend + Frontend') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    sh "docker build -t ${BACKEND_IMAGE}:${BACKEND_IMAGE_TAG} ."
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    sh "docker build -t ${FRONTEND_IMAGE}:${FRONTEND_TAG} ."
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Backend réussi !"
        }

        failure {
            echo "❌ Le pipeline a échoué, vérifiez les logs Jenkins."
        }
    }
}
