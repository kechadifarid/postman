pipeline {
    agent any

    stages {

        stage('global stage') {

            agent {
                docker {
                    image 'postman/newman:latest'
                    args '-u root --entrypoint='
                }
            }

            stages {
                stage('install allure') {
                    steps {
                        sh 'npm install -g newman-reporter-allure'
                    }
                }

                stage('clean allure results') {
                    steps {
                        sh '''
                            echo "Suppression du cache Allure..."
                            rm -rf allure-results
                            mkdir -p allure-results
                            echo "Dossier allure-results nettoyé avec succès"
                        '''
                    }
                }

                stage('run avec new man') {
                    steps {
                        sh 'newman run collection.json -e env.json --reporters cli,allure --reporter-allure-resultsDir allure-results'
                        sh 'chmod -R a+rwX allure-results'
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