pipeline {
    agent any
    
    environment {
        // Define the SonarQube server URL and token
        SONARQUBE_SERVER = 'http://192.168.10.116:9000/'
        SONARQUBE_TOKEN = 'sqp_e08347e9c102dacba7f324d6600d9b90a085aad7'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/genegenie07/Vulnerable-Web-Application.git'
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
                        -Dsonar.host.url=${env.SONARQUBE_SERVER} \
                        -Dsonar.login=${env.SONARQUBE_TOKEN} \
                        -Dsonar.verbose=true -X
                        """
                    }
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
