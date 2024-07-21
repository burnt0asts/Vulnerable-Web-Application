pipeline { 
    agent any 
    stages { 
        stage('Checkout') { 
            steps { 
                git branch: 'master', url: 'https://github.com/burnt0asts/Vulnerable-Web-Application.git' 
            } 
        } 
  
        stage('Code Quality Check via SonarQube') { 
            steps { 
                script { 
                    def scannerHome = tool 'SonarQube'; 
                    withSonarQubeEnv('SonarQube') { 
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
                    } 
                } 
            } 
        } 
    } 
    post { 
        always { 
            script {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        } 
    } 
}
