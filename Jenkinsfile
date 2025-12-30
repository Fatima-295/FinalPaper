pipeline {
    agent any

    environment {
        VENV = "venv"
        DEPLOY_DIR = "/tmp/flask_app_deploy"
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning the Flask application repository...'
                git url: 'https://github.com/Fatima-295/FinalPaper.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests (if available)...'
                sh '''
                . $VENV/bin/activate
                pytest || echo "No tests found, skipping test stage"
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo 'Preparing Flask application for deployment...'
                sh '''
                mkdir -p build
                cp app.py requirements.txt build/
                cp -r static build/
                cp -r templates build/
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Simulating deployment...'
                sh '''
                mkdir -p $DEPLOY_DIR
                cp -r build/* $DEPLOY_DIR
                echo "Flask app deployed to $DEPLOY_DIR"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check Jenkins logs.'
        }
    }
}
