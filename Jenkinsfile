pipeline { 
    agent any 
    stages { 
        stage ('Checkout') { 
            steps { 
                git branch: 'master', url: 'https://github.com/burnt0asts/Vulnerable-Web-Application' 
            } 
        }
        stage('Code Quality Check via SonarQube') { 
            steps { 
                script { 
                    def scannerHome = tool 'SonarQube'; 
                    withSonarQubeEnv('SonarQube') { 
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=sqp_99aef2bff1013cef099f1a0d9a149bfb13c86c9c
                        """
                    } 
                } 
            } 
        } 
    } 
    post { 
        always { 
            script {
                def sonarTask = waitForQualityGate()
                if (sonarTask.status != 'OK') {
                    unstable("Pipeline failed due to quality gate failure: ${sonarTask.status}")
                }
            }
        }
    } 
}
