pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo '========== CHECKOUT =========='
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '========== INSTALL DEPENDENCIES =========='
                bat '''
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo '========== RUNNING PYTEST =========='
                bat '''
                pytest test_app.py ^
                --html=reports/report.html ^
                --self-contained-html ^
                --junitxml=reports/results.xml
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '========== BUILDING DOCKER IMAGE =========='
                bat '''
                docker build -t jenkins-ai-pipeline:latest .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '========== RUNNING DOCKER CONTAINER =========='
                bat '''
                docker run --rm jenkins-ai-pipeline:latest
                '''
            }
        }
    }

    post {

        always {
            echo '========== ARCHIVING REPORTS =========='

            archiveArtifacts artifacts: 'reports/*', fingerprint: true

            junit 'reports/results.xml'
        }

        success {
            echo '========================================='
            echo 'Pipeline completed successfully.'
            echo '========================================='
        }

        failure {
            echo '========================================='
            echo 'Pipeline failed.'
            echo '========================================='
        }
    }
}
