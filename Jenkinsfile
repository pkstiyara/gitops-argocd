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

        // stage('Push Docker Image'){
        //     steps{
        //         script{

        //             docker.withRegistry('',REGISTRY_CREDS){
        //                 docker_image.push("$BUILD_NUMBER")
        //                 docker_image.push('latest')
        //             }
        //         }
        //     }
        // }
    }
}