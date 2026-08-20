pipeline {
    agent {
        docker {
            image 'postman/newman:latest'
            args '-u root --entrypoint='
        }
    }

    stages {
        stage('install deps') {
            steps {
                sh 'npm install --save-dev newman-reporter-allure allure-commandline'
                sh 'apk add --no-cache openjdk11-jre'   // <-- ajoute Java pour allure-commandline
            }
        }
        stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json --reporters cli,allure --reporter-allure-resultsDir allure-results'
            }
        }
        stage('generer rapport allure') {
            steps {
                sh 'npx allure generate allure-results -c -o allure-report'
                sh 'chmod -R a+rwX allure-results allure-report'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'allure-results/**, allure-report/**', allowEmptyArchive: true
        }
    }
}