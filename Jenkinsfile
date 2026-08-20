pipeline {
    agent any   // agent Jenkins "normal", pas le conteneur docker

    stages {
        stage('install allure') {
            agent {
                docker { image 'postman/newman:latest'; args '-u root --entrypoint=' }
            }
            steps {
                sh 'npm install --save-dev newman-reporter-allure'
            }
        }
        stage('lancer test avec newman') {
            agent {
                docker { image 'postman/newman:latest'; args '-u root --entrypoint=' }
            }
            steps {
                sh 'newman run collection.json -e env.json --reporters cli,allure --reporter-allure-resultsDir allure-results'
                sh 'chmod -R a+rwX allure-results'
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