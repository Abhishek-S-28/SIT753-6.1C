pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Simulate SonarQube scanner
                echo 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                // Simulate OWASP ZAP scan
                echo 'zap-cli quick-scan --self-contained -o "-config api.disablekey=true" -t http://localhost:8080'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Simulate deployment to a staging server
                echo 'scp target/myapp.jar user@staging-server:/path/to/deployment'
                echo 'ssh user@staging-server "java -jar /path/to/deployment/myapp.jar"'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Simulate a simple HTTP check to the deployed application
                echo 'curl http://staging-server:8080/health || exit 1'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Simulate production deployment steps
                echo 'scp target/myapp.jar user@production-server:/path/to/deployment'
                echo 'ssh user@production-server "java -jar /path/to/deployment/myapp.jar"'
            }
        }
    }
    post {
        always {
            mail to: 'abhisheksrini2001@gmail.com',
                 subject: "Jenkins Pipeline Status: ${currentBuild.fullDisplayName}",
                 body: "Pipeline run finished. Check the Jenkins dashboard for details."
                 // Note: The attachLog parameter was incorrect and has been commented out
                 // attachmentsPattern: '**/target/*.log'
        }
    }
}
