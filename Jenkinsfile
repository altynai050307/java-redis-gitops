pipeline {
    agent any
    stages {
        stage('Code Quality (SonarQube)') {
            steps {
                script {
                    // SonarQube талдауын жүргізу
                    withSonarQubeEnv('SonarQube-Server') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Қауіпсіздік стандартынан өтпесе тоқтату
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Security Scan (Trivy)') {
            steps {
                sh 'trivy image --exit-code 1 --severity CRITICAL,HIGH myuser/java-app:latest'
            }
        }
    }
}
