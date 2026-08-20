pipeline {
    agent any

    stages {
        stage('install allure') {
            steps {
                script {
                    docker.image('postman/newman:latest').inside('-u root --entrypoint=') {
                        sh 'npm install --save-dev newman-reporter-allure'
                    }
                }
            }
        }
        stage('lancer test avec newman') {
            steps {
                script {
                    docker.image('postman/newman:latest').inside('-u root --entrypoint=') {
                        sh 'newman run collection.json -e env.json --reporters cli,allure --reporter-allure-resultsDir allure-results'
                       // sh 'chmod -R a+rwX allure-results'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'allure-results/*', allowEmptyArchive: true
            allure includeProperties: false,
                   jdk: '',
                   results: [[path: 'allure-results/']]
        }
    }
}