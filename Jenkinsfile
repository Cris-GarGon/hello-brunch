#!usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
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
            sh 'trivy filesystem --format json --output trivy-fs.json .'
                sh 'trivy image --format json --output trivy-image.json hello-brunch'
            }
            post {
                always {
                    recordIssues(
                        enabledForFailure: true,
                        aggregatingResults:true,
                        tool: trivy(pattern: 'trivy-*.json')
                    )
                }
            }

	}
    }
}

