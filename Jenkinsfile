pipeline {
    agent any

    stages {
        stage('Check Tools') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
            }
        }
    }
}
