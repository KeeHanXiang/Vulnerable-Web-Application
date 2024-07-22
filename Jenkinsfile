pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/KeeHanXiang/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://<your-sonarqube-server>:9000 -Dsonar.login=<your-sonarqube-token>"
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
