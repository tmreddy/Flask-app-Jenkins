pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
    }

    stages {

        stage('Checkout') {
            steps {
                // Clean workspace before checkout
                deleteDir()
                // Clone the repo
                git branch: 'main', url: 'https://github.com/tmreddy/Python-Jenkins.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh """
                    # Create virtual environment if it doesn't exist
                    python3 -m venv ${VENV_DIR}
                    # Upgrade pip inside the venv
                    ${VENV_DIR}/bin/pip install --upgrade pip
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                sh """
                    # Install dependencies from requirements.txt
                    ${VENV_DIR}/bin/pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                    # Run pytest and generate JUnit XML report
                    ${VENV_DIR}/bin/pytest --junitxml=report.xml
                """
            }
        }

        stage('Package') {
            steps {
                sh 'tar --exclude-vcs --exclude="*.pyc" -czf app.tar.gz *'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'app.tar.gz', fingerprint: true
        }
    }
}
