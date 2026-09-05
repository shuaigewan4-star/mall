pipeline {

    agent {
        label 'jenkins-agent'
    }


    stages {

        stage('Agent Test') {
            steps {
                sh '''
                hostname
                java -version
                '''
            }
        }


        stage('Maven Test') {

            steps {

                container('maven') {

                    sh '''
                    mvn -version
                    '''

                }

            }
        }

    }
}
