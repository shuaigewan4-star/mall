pipeline {

    agent {
        label 'jenkins-agent'
    }


    stages {

        stage('Build mall-admin') {

            steps {

              container('maven') {  
                sh '''
                mvn clean package \
                -pl mall-admin \
                -am \
                -Ddocker.host=unix:///var/run/docker.sock \
                -Ddocker.skip=true
                '''
            }
        }
    }

        stage('Build mall-portal') {

            steps {

              container('maven') {
                sh '''
                mvn clean package \
                -pl mall-portal \
                -am \
                -Ddocker.host=unix:///var/run/docker.sock \
                -Ddocker.skip=true
                '''
            }
        }
    }

        stage('Build mall-search') {

            steps {

              container('maven') {
                sh '''
                mvn clean package \
                -pl mall-search \
                -am \
                -Ddocker.host=unix:///var/run/docker.sock \
                -Ddocker.skip=true
                '''
            }
        }
    }

  }
}
