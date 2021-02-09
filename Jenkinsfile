#!usr/bin/env groovy
pipeline {
    agent any
    /*options {
        ansiColor('xterm')
    }*/
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d'
            }
        }

	stage('security'){
	   steps {
                sh 'trivy image --format json --output trivy-results.json hello-brunch'
            }
            post {
                always {
                    recordIssues(
                        enabledForFailure: true,
                        tool: trivy(pattern: 'trivy-results.json')
                    )
                }
            }

	}
    }
}

