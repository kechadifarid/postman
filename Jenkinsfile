pipeline {
    agent {
        docker { image 'postman/newman:latest' }
    }
    stages {
        stage('lancer test avec newman') {
            steps {
                sh 'newman run collection.json -e env.json'
            }
        }
    }
}