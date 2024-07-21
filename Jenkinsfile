pipeline {
    agent any
    tools {
        // Ensure the SonarQube scanner is available
        sonarQube 'SonarQube'
        sonarScanner 'SonarQube' // Adjust to the correct tool name as per your Jenkins setup
    }
    environment {
        // SonarQube server URL and token can be defined as environment variables
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'sqp_99aef2bff1013cef099f1a0d9a149bfb13c86c9c'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/burnt0asts/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }
    post {
        always {
            script {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
