pipeline {
    agent any

    stages {
        stage('Build Backends') {
            parallel {
                stage('Backend Service') {
                    steps {
                        // Using the Maven wrapper included in the project
                        bat 'cd backend && mvnw.cmd clean package -DskipTests'
                    }
                }
                stage('Gateway Service') {
                    steps {
                        // Using the Maven wrapper included in the project
                        bat 'cd gateway && mvnw.cmd clean package -DskipTests'
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                // Assumes 'npm' is in the system PATH
                bat 'cd frontendweb && npm install && npm run build'
            }
        }

        stage('Setup ML') {
            steps {
                // Assumes 'pip' or 'python' is in the system PATH
                bat 'cd machine_learning && pip install -r requirements.txt'
            }
        }
    }
}
