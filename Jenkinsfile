pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                // Install dependencies using pip (Windows)
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                bat 'python -m unittest discover -s .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application (copy app.py to deploy folder)...'
                bat '''
                if not exist "%WORKSPACE%\\python-app-deploy" mkdir "%WORKSPACE%\\python-app-deploy"
                copy /Y "%WORKSPACE%\\app.py" "%WORKSPACE%\\python-app-deploy\\"
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Starting Flask application in background...'
                bat '''
                cd "%WORKSPACE%\\python-app-deploy"
                start /B python app.py
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Running functional test (same unittest again)...'
                bat 'python test_app.py'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
