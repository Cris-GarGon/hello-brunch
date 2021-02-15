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
        stage('Publish'){
            steps {
                withDockerRegistry(credentialsId: 'gitlab-registry', url: 'http://10.250.2.6:5050') {
                    //sh 'docker tag hello-brunch:latest 10.250.2.6:5050/root/hello-brunch:latest'
                    sh 'docker tag hello-brunch:latest 10.250.2.6:5050/root/hello-brunch:BUILD-1.${BUILD_ID}'
                    //sh 'docker push 10.250.2.6:5050/root/hello-brunch:latest
                    sh 'docker push 10.250.2.6:5050/root/hello-brunch:BUILD-1.${BUILD_ID}'
                }

                sshagent(credentials:['gitlab_ssh']) {
                    sh 'git tag BUILD-1.${BUILD_ID}'
                    sh 'git push --tags'
                }
            }
        }
        stage('Deploy'){
            steps {
                sshagent(credentials:['deploy-ssh']) {
                    sh ¡ssh -t -o "StrictHostKeyChecking no" deploy@10.250.2.6 'docker-compose pull & docker-compose up -d''

                }
            }
        }
    }
}

