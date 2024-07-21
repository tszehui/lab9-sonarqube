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
                        sh """
                            echo "SonarQube Scanner Home: ${scannerHome}"
                            echo "SonarQube Server URL: http://your-sonarqube-server:9000"
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://your-sonarqube-server:9000 \
                            -Dsonar.login=${env.SonarQube} \
                            -X
                        """
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
