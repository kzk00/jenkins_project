pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHON_PATH = 'C:\\Users\\kobei\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'  // Adjust the path as needed
        PYTHONIOENCODING = 'utf-8'  // Set encoding to utf-8
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "${env.PYTHON_PATH} -m venv ${VIRTUAL_ENV}"
                    }
                    bat """
                        call ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m pip install -r requirements.txt
                    """
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat """
                        call ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m flake8 app.py
                    """
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat """
                        call ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m pytest
                    """
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat """
                        call ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage run -m pytest
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage report
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage html
                    """
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat """
                        call ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate
                        ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m bandit -r . -f txt -o bandit_report.txt
                    """
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
