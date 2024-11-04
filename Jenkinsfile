pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "${env.PYTHON_PATH} -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage run -m pytest"
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage report"
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage html"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\python.exe -m bandit -r . || true"
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
