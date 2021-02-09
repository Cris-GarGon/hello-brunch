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
                sh 'trivy docker-compose --format json --output trivy-results.json'
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

