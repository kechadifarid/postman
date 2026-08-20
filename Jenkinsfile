pipeline {

    agent none

    stages {

        stage('Tests Newman') {

            agent {
                docker {
                    image 'postman/newman:latest'
                    args '-u root --entrypoint='
                }
            }

            stages {

                stage('Clean Allure results') {
                    steps {
                        sh '''
                            rm -rf allure-results
                            mkdir -p allure-results
                        '''
                    }
                }

                stage('Install Allure reporter') {
                    steps {
                        sh '''
                            npm install -g newman-reporter-allure

                            echo "Reporter installé :"
                            npm list -g newman-reporter-allure
                        '''
                    }
                }

                stage('Run Newman') {
                    steps {
                        sh '''
                            newman run collection.json \
                                -e env.json \
                                --reporters cli,allure \
                                --reporter-allure-resultsDir allure-results

                            echo "Résultats Allure :"
                            ls -lah allure-results
                        '''
                    }
                }
            }
        }

        stage('Préparer les résultats') {

            agent any

            steps {
                sh '''
                    echo "Correction des permissions..."

                    chown -R $(id -u):$(id -g) allure-results || true

                    ls -lah allure-results
                '''
            }
        }

        stage('Générer le rapport Allure') {

            agent any

            steps {

                allure(
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'allure-results']]
                )
            }
        }
    }

    post {

        always {

            archiveArtifacts(
                artifacts: 'allure-results/**',
                allowEmptyArchive: true
            )
        }
    }
}
