pipeline {
    agent any
    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQube' // Ensure this matches the configured scanner name
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/burnt0asts/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure this matches the configured server name
                    sh """
                        ${env.SONARQUBE_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://sonarqube:9000 \
                        -Dsonar.login=sqp_2ee303a5d8d32019b1d5ac43350472d80a6fa6d1
                    """
                }
            }
        }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}