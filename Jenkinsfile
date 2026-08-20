pipeline {
    agent {
        docker { 
                image 'postman/newman:latest' 
                args '-u root --entrypoint='
            }
    }
    stages {
        stage('global stage'){
            
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.62.1-noble'
                    args '-u root --entrypoint='
                }
            }
            stages{
                stage('install deps'){
                    steps{
                        sh 'npm ci'
                    }
                }

                stage('clean allure results'){
                    
                    steps{
                        sh '''
                            echo "Suppression du cache Allure..."
                            rm -rf allure-results
                            mkdir -p allure-results
                            echo "Dossier allure-results nettoyé avec succès"
                        '''
                    }
                }
            }
            stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json   --reporters cli,allure   --reporter-allure-resultsDir output/allure-results'
            }
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