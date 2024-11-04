pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHON_PATH = 'C:\\Users\\kobei\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'  // Replace with your Python path
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    // Create a virtual environment if it doesn't exist
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "${env.PYTHON_PATH} -m venv ${VIRTUAL_ENV}"
                    }
                    // Activate the virtual environment and install requirements
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    // Run flake8 in the virtual environment
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run pytest in the virtual environment
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && ${env.PYTHON_PATH} -m pytest"
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
