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
            sh 'npm install --save-dev newman-reporter-allure'

        }
            stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json   --reporters cli,allure   --reporter-allure-resultsDir output/allure-results'
            }
        }
        
    }

    post{
        always{
            script{
                if(params.ALLURE){
                    unstash 'allure-results'
                    archiveArtifacts 'allure-results/*'
                    allure includeProperties: false,
                           jdk: '',
                           results: [[path: 'allure-results/']]
                }
            }
        }
    }
}