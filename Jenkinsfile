pipeline {
    agent any

    stages {
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
                        -d DDT4reqres.json ^
                        --insecure ^
                        --verbose ^
                        --reporters cli,html,htmlextra ^
                        --reporter-html-export results\\report-%BUILD_NUMBER%-html.html ^
                        --reporter-htmlextra-export results\\report-%BUILD_NUMBER%-htmlextra.html
                '''
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                reportDir: 'results',
                reportFiles: 'report-%BUILD_NUMBER%-html.html',
                reportName: 'Postman DDT Report (HTML)'
            ])
             publishHTML(target: [
                reportDir: 'results',
                reportFiles: 'report-%BUILD_NUMBER%-htmlextra.html',
                reportName: 'Postman DDT Report (HTMLEXTRA)'
            ])
        }
    }
}
