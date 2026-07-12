pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Unit Tests') {
            steps {
                bat 'pytest test_app.py --html=reports/report.html --self-contained-html'
            }
        }

    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/*', fingerprint: true
        }
    }
}
