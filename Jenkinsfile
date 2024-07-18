pipeline {
    agent any
    tools {
        // Specify the tool to use for SonarQube
        sonarQube 'SonarQube'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the Git repository
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        // Run SonarQube scanner
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
                    }
                }
            }
        }
    }
    post {
        always {
            // Record issues from SonarQube analysis
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}
