pipeline {
    agent any

    tools {
        // Optional: Use NodeJS plugin if configured in Jenkins
        // nodejs 'Node18'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                bat '''
                    npm install -g newman
                    npm install -g newman-reporter-htmlextra
                '''
            }
        }

        stage('Run Newman Tests') {
            steps {
                bat '''
                    if exist results rmdir /s /q results
                    mkdir results

                    newman run reqres_API_DDT.postman_collection.json ^
                        -e ReqRes.postman_environment.json ^
                        -d data.csv ^
                        --insecure ^
                        --reporters cli,html,htmlextra ^
                        --reporter-html-export results\\report-html.html ^
                        --reporter-htmlextra-export results\\report-htmlextra.html
                '''
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                reportDir: 'results',
                reportFiles: 'report-html.html',
                reportName: 'Postman DDT Report (HTML)'
            ])
            publishHTML(target: [
                reportDir: 'results',
                reportFiles: 'report-htmlextra.html',
                reportName: 'Postman DDT Report (HTMLEXTRA)'
            ])
        }
    }
}
