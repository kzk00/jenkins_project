pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHON_PATH = 'C:\\Users\\kobei\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'  // Set the path to your Python executable
        PYTHONPATH = "${env.WORKSPACE}"
        PYTHONIOENCODING = 'utf-8'  // Set UTF-8 encoding for all Python output
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "${PYTHON_PATH} -m venv ${VIRTUAL_ENV}"
                    }
                    bat """
                        ${VIRTUAL_ENV}\\Scripts\\activate
                        && ${PYTHON_PATH} -m pip install -r requirements.txt
                    """
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m pip freeze"  // List installed packages
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m pytest"
                }
            }
        }
        stage('Code Coverage') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m pip install coverage"  // Ensure coverage is available
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m coverage run -m pytest"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m coverage report"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && ${PYTHON_PATH} -m coverage html"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
