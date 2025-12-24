pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = 'docker-compose'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo 'Building Docker images...'
                    // Build services using docker-compose build
                    // This assumes docker-compose is installed on the Jenkins agent
                    if (isUnix()) {
                        sh 'docker-compose build'
                    } else {
                        bat 'docker-compose build'
                    }
                }
            }
        }

        stage('Test Composition') {
            steps {
                script {
                    echo 'Testing Docker Composition...'
                    try {
                        // Start up the services in detached mode
                        if (isUnix()) {
                            sh 'docker-compose up -d'
                            // Wait a bit for services to start
                            sleep 30
                            // Check if containers are running
                            sh 'docker-compose ps'
                        } else {
                            bat 'docker-compose up -d'
                            sleep 30
                            bat 'docker-compose ps'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    } finally {
                        // Clean up
                        echo 'Cleaning up...'
                        if (isUnix()) {
                            sh 'docker-compose down'
                        } else {
                            bat 'docker-compose down'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build and Test succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
