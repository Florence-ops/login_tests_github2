pipeline {
    agent any
    parameters {
        string(defaultValue: "https://github.com/Florence-ops/tests_github.git", description: 'GitHub Repository URL', name: 'GITHUB_URL')
    }
    tools {
        maven 'Maven_3.9.8' // Ensure this matches your Maven installation name in Jenkins
        jdk 'JDK_11' // Ensure this matches your JDK installation name in Jenkins
    }
    stages {
        stage('Checkout Git repository') {
            steps {
                git branch: 'main', url: "${params.GITHUB_URL}"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Verify Report Generation') {
            steps {
                // List the directory structure to verify the location of test report files
                sh 'ls -R target'
                sh 'cat target/surefire-reports/*' // Optional: Print contents of report files to verify
            }
        }
        stage('Report') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}


