pipeline{
    agent any

    environment{
        DOCKERHUB_USERNAME = "pkstiyara"
        APP_NAME = "gitops-argo-cd"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
        }

    stages{

        stage("Cleanup the workspacke"){

            steps{
                script{
                    cleanWs()
                }
            
            }
        }

        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsID: 'github',
                    url: 'https://github.com/pkstiyara/gitops-argocd.git',
                    branch: 'main'
                }
            }
        }

        stage('Build Docker Images'){
            steps{
                script {
                     docker_image = docker.build("${IMAGE_NAME}", "-f Dockerfile .")
                }
            }

        }

        stage('Push Docker Image'){
            steps{
                script{

                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Updating kubernetes Deployment'){
            steps{
                script{
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }

        }
        // stage('Push the changed deployment file to Git'){
        //     steps{
        //         script{
        //             sh '''
        //         }
        //     }
        // }


    }
}