pipeline {
    agent {
        docker { 
                image 'postman/newman:latest' 
                args '-u root --entrypoint='
            }
    }
    
    stages {
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
        stage('install allure') {
    steps {
        sh '''
            npm install -g newman-reporter-allure

            echo "Reporter Allure installé :"
            npm list -g newman-reporter-allure
        '''
    }
}
            stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json   --reporters cli,allure   --reporter-allure-resultsDir allure-results'
            }
        }
        
    }


}