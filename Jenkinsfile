pipeline {
            agent {
        docker { 
                image 'postman/newman:latest' 
                args '-u root --entrypoint='
            }
    }   
    stages {
        stage('install allure ')
        {
            steps{
            sh 'npm install --save-dev newman-reporter-allure'
            }

        }
            stage('lancer test avec newman') {
                     steps {
                         sh 'newman run collection.json -e env.json --reporters cli,allure --reporter-allure-resultsDir allure-results'
                         sh 'chmod -R a+rwX allure-results'   // <-- fix: make files writable by non-root Jenkins user
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