pipeline {

    agent {
        label 'jenkins-agent'
    }

    environment {
       REGISTRY = '192.168.0.198'
        IMAGE_TAG = "${BUILD_NUMBER}"
 
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
        stage('Docker Build mall-admin') {
            steps {

              container('maven') {
                sh '''
                docker build \
                -t ${REGISTRY}/mall/mall-admin:${IMAGE_TAG} \
                -f docker/mall-admin/Dockerfile .
                '''
            }
       }
    }

stage('Docker Push mall-admin') {

    steps {

        container('maven') {

            withCredentials([
                usernamePassword(
                    credentialsId: 'harbor-credential',
                    usernameVariable: 'HARBOR_USER',
                    passwordVariable: 'HARBOR_PASS'
                )
            ]) {

                sh '''
                echo $HARBOR_PASS | docker login 192.168.0.198 \
                -u $HARBOR_USER \
                --password-stdin

                docker push ${REGISTRY}/mall/mall-admin:${IMAGE_TAG}
                '''

            }

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

        stage('Docker Build mall-portal') {
            steps {

              container('maven') {
                sh '''
                docker build \
                -t 192.168.0.198/mall/mall-portal:v1 \
                -f docker/mall-portal/Dockerfile .
                '''
            }
       }
    }

stage('Docker Push mall-portal') {

    steps {

        container('maven') {

            withCredentials([
                usernamePassword(
                    credentialsId: 'harbor-credential',
                    usernameVariable: 'HARBOR_USER',
                    passwordVariable: 'HARBOR_PASS'
                )
            ]) {

                sh '''
                echo $HARBOR_PASS | docker login 192.168.0.198 \
                -u $HARBOR_USER \
                --password-stdin

                docker push 192.168.0.198/mall/mall-portal:v1
                '''

            }

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


        stage('Docker Build mall-search') {
            steps {

              container('maven') {
                sh '''
                docker build \
                -t 192.168.0.198/mall/mall-search:v1 \
                -f docker/mall-search/Dockerfile .
                '''
            }
       }
    }
stage('Docker Push mall-search') {

    steps {

        container('maven') {

            withCredentials([
                usernamePassword(
                    credentialsId: 'harbor-credential',
                    usernameVariable: 'HARBOR_USER',
                    passwordVariable: 'HARBOR_PASS'
                )
            ]) {

                sh '''
                echo $HARBOR_PASS | docker login 192.168.0.198 \
                -u $HARBOR_USER \
                --password-stdin

                docker push 192.168.0.198/mall/mall-search:v1
                '''

            }

        }

    }

}

  }
}
