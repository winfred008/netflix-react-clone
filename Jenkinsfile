pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("905418365451.dkr.ecr.us-east-2.amazonaws.com/netflix-jan:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 905418365451.dkr.ecr.us-east-2.amazonaws.com/netflix-jan:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://905418365451.dkr.ecr.us-east-2.amazonaws.com/netflix-jan', 'ecr:us-east-2:winfred008-ecr') {
                    // build image
                    def myImage = docker.build("905418365451.dkr.ecr.us-east-2.amazonaws.com/netflix-jan:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
