pipeline {

     agent {
        label 'agent-windows'  // ou 'linux' selon votre agent Jenkins
    }

    environment {
        DOCKERHUB_USER = "ditdevops1"
        BACKEND_IMAGE  = "${DOCKERHUB_USER}/backend-gestion-employe-l2dit"
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/frontend-gestion-employe-l2dit"

        BACKEND_IMAGE_TAG = "1.${BUILD_NUMBER}"
        FRONTEND_TAG      = "1.${BUILD_NUMBER}"
    }

    stages {

        // Checkout code source Backend + Frontend
        stage('Checkout Backend + Frontend') {
            steps {
                checkout scm
            }
        }

        // Build Backend Docker Image
        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    script {
                        bat "docker build -t ${BACKEND_IMAGE}:${BACKEND_IMAGE_TAG} ."
                    }
                }
            }
        }

        // Build Frontend Docker Image
        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    script {
                        bat "docker build -t ${FRONTEND_IMAGE}:${FRONTEND_TAG} ."
                    }
                }
            }
        }

        // Déployer les services avec Docker Compose en mode détaché
        stage('Deploy with Docker Compose') {
            steps {
                bat "docker compose up -d --build"
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
