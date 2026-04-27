pipeline {
    agent any

    environment {
        PACKAGE_NAME = "my_package"
        PYTHON       = "python3"
        VENV_DIR     = "venv"
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo '🐍 Creating virtual environment...'
                sh """
                    ${PYTHON} -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                """
            }
        }


        stage('Build WHL Package') {
            steps {
                echo '📦 Building wheel package...'
                sh """
                    . ${VENV_DIR}/bin/activate
                    python -m build
                """
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo '🗄️ Archiving dist artifacts...'
                archiveArtifacts artifacts: 'dist/*.whl, dist/*.tar.gz',
                                 fingerprint: true,
                                 allowEmptyArchive: false
            }
        }

        }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs above.'
        }
        always {
            echo '🧹 Cleaning up workspace...'
            cleanWs()
        }
    }
}

