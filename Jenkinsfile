pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHON_PATH = 'C:\\Users\\kobei\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'  // Adjust the path as needed
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "${env.PYTHON_PATH} -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m coverage run -m pytest"
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m coverage report"
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m coverage html"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m bandit -r ."
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
