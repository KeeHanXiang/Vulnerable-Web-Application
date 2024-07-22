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
                    def scannerHome = tool 'SonarQube'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://172.29.112.1:9000 -Dsonar.login=sqp_13b975025a0a43721da78ea62f99cb77a1ccb593"
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
