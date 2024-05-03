pipeline {
    agent any

    tools {
        // Make sure Maven and JDK are configured in Jenkins Global Tool Configuration
        maven 'Maven 3.6.3'
        jdk 'JDK 11'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    // SonarQube scanner needs to be preconfigured in Jenkins
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // OWASP ZAP scan, assumes ZAP is installed and set up to run as CLI
                    sh 'zap-cli quick-scan --self-contained -o "-config api.disablekey=true" -t http://localhost:8080'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    // Assumes SSH keys are set up for Jenkins to access the staging server
                    sh 'scp target/myapp.jar user@staging-server:/path/to/deployment'
                    sh 'ssh user@staging-server "java -jar /path/to/deployment/myapp.jar"'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    // Example of a simple HTTP check to the deployed application
                    sh 'curl http://staging-server:8080/health || exit 1'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    // Production deployment steps
                    sh 'scp target/myapp.jar user@production-server:/path/to/deployment'
                    sh 'ssh user@production-server "java -jar /path/to/deployment/myapp.jar"'
                }
            }
        }
    }
    post {
        always {
            mail to: 'abhisheksrini2001@gmail.com',
                 subject: "Jenkins Pipeline Status: ${currentBuild.fullDisplayName}",
                 body: "Pipeline run finished. Check the Jenkins dashboard for details.",
                 attachLog: true
        }
    }
}
