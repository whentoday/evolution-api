pipeline {
    agent {
        kubernetes {
            yamlFile 'kaniko-builder.yaml'
        }
    }
    
    environment {
        DOCKER_REGISTRY = "containerregistry.bytefy.com.br"
        DOCKER_IMAGE = "bytefy/evolution-wpp"
        DOCKERFILE = "Dockerfile"
        
        RANCHER_URL = "https://rancher.bytefy.com.br:8443/v2"
        RANCHER_TOKEN = credentials('token-rancher') 

        CLUSTER_ID = 'c-m-tdrxlzqp'
        PROJECT_ID = 'p-rhwch'
        NAMESPACE = 'bytefy'
        DEPLOYMENT = 'bytefy'
        SHOW_ERROR_LOGS = 'true' // Change to 'false' if you don't want to print error logs
    }
    
    stages {
        stage('Print Pod Name') {
            steps {
                script {
                    echo "O pipeline está sendo executado no pod: $HOSTNAME"
                }
            }
        }
        
        stage('Build & Push with Kaniko') {
            steps {
                container(name: 'kaniko', shell: '/busybox/sh') {
                    withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            # Executa o Kaniko
                            /kaniko/executor --dockerfile `pwd`/${DOCKERFILE} --context `pwd` --destination=${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER} --destination=${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }

    }
}
