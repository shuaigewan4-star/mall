pipeline {

    agent {
        label 'jenkins-agent'
    }

    environment {
        REGISTRY = '192.168.0.198'
        NAMESPACE = 'mall-prod'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build & Push Images') {

            steps {

                container('maven') {

                    script {

                        def services = [
                            'mall-admin',
                            'mall-portal',
                            'mall-search'
                        ]

                        for (service in services) {

                            echo "===== Build ${service} ====="

                            sh """
                            mvn clean package \
                            -pl ${service} \
                            -am \
                            -Ddocker.host=unix:///var/run/docker.sock \
                            -Ddocker.skip=true
                            """

                            echo "===== Docker Build ${service} ====="

                            sh """
                            docker build \
                            -t ${REGISTRY}/mall/${service}:${IMAGE_TAG} \
                            -f docker/${service}/Dockerfile .
                            """

                            echo "===== Docker Push ${service} ====="

                            withCredentials([
                                usernamePassword(
                                    credentialsId: 'harbor-credential',
                                    usernameVariable: 'HARBOR_USER',
                                    passwordVariable: 'HARBOR_PASS'
                                )
                            ]) {

                                sh """
                                echo \$HARBOR_PASS | docker login ${REGISTRY} \
                                -u \$HARBOR_USER \
                                --password-stdin

                                docker push ${REGISTRY}/mall/${service}:${IMAGE_TAG}
                                """
                            }
                        }
                    }
                }
            }
        }

        stage('Deploy mall-admin') {

            steps {

                container('maven') {

                    sh '''
                    helm upgrade mall-admin ./mall-helm/mall-admin \
                    -n ${NAMESPACE} \
                    --set image.tag=${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Rollout mall-admin') {

            steps {

                container('maven') {

                    sh '''
                    kubectl rollout status deployment mall-admin \
                    -n ${NAMESPACE}
                    '''
                }
            }
        }
    }
}

