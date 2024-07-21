pipeline { 
    agent any 
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_AUTH_TOKEN = credentials('SonarQube')
    }
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
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_AUTH_TOKEN} \
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
