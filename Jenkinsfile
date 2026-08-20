pipeline {
    agent any
   
    stages {
            agent {
        docker { 
                image 'postman/newman:latest' 
                args '-u root --entrypoint='
            }
    }
        stage('install allure ')
        {
            steps{
            sh 'npm install --save-dev newman-reporter-allure'
            }

        }
            stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json   --reporters cli,allure   --reporter-allure-resultsDir allure-results'
                stash name: 'allure-results', includes: 'allure-results/*'
            }
        }
        
    }

    post{
        always{
            script{
                
                    unstash 'allure-results'
                    archiveArtifacts 'allure-results/*'
                    allure includeProperties: false,
                           jdk: '',
                           results: [[path: 'allure-results/']]
                }
            
        }
    }
}