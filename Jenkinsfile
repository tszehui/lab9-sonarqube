pipeline { 
    agent any 
    stages { 
        stage ('Checkout') { 
            steps { 
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git' 
            } 
        }

        stage('Code Quality Check via SonarQube') { 
            steps { 
                script { 
                    def scannerHome = tool 'SonarQube'; 
                    withSonarQubeEnv('SonarQube') { 
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.token=sqp_95f5b4ca2a747c9236d5181916fb90ba663dbd15"
                    } 
                } 
            } 
        } 
    } 
    post { 
        always { 
            recordIssues enabledForFailure: true, tools: [sonarQube()] 
        } 
    } 
}
