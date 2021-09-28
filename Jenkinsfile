pipeline {
    agent any
    stages {
        stage('Build') {

            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy to staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to production') {
            steps {
                timeout(time: 5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'/*, submitter: admin*/
                }
                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo 'Code successfully deployed in Production'
                }
                failure {
                    echo 'Deployment failed'
                }
            }
        }
    }
}
