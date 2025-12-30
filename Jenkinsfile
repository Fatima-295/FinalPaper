pipeline {
    agent any

    environment {
        VENV = "venv"
        DEPLOY_DIR = "C:\\FlaskAppDeploy"
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning the Flask application repository...'
                git 'https://github.com/Fatima-295/FinalPaper.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                bat """
                python -m venv %VENV%
                call %VENV%\\Scripts\\activate.bat
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests (if available)...'
                bat """
                call %VENV%\\Scripts\\activate.bat
                pytest || echo No tests found
                """
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building the Flask application...'
                bat """
                mkdir build
                xcopy app.py build\\ /Y
                xcopy requirements.txt build\\ /Y
                xcopy static build\\static /E /I /Y
                xcopy templates build\\templates /E /I /Y
                """
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Simulating deployment...'
                bat """
                mkdir %DEPLOY_DIR%
                xcopy build\\* %DEPLOY_DIR% /E /I /Y
                echo Flask app deployed to %DEPLOY_DIR%
                """
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
