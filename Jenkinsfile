pipeline {
    agent any

    stages {
        stage('Install Newman') {
            steps {
                bat 'npm install -g newman'
            }
        }

        stage('Run Newman Tests') {
            steps {
                bat '''
                    mkdir results

                    newman run reqres_API_DDT.postman_collection.json ^
                        -e ReqRes.postman_environment.json ^
                        -d DDT4reqres.json ^
                        --insecure ^
                        --reporters cli,html ^
                        --reporter-html-export results\report.html
                '''
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                reportDir: 'results',
                reportFiles: 'report.html',
                reportName: 'Postman DDT Report'
            ])
        }
    }
}
