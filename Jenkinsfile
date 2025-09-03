pipeline {
    agent any
    environment {
        IMAGE_NAME = 'butternoei008/mallow-sale-api'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main',
                   credentialsId: 'github-credentials',
                   url: 'https://github.com/hifat/mallow-sale-k8s-manifests'
            }
        }

        stage('Update the Deployment Tags') {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's|image: ${IMAGE_NAME}:.*|image: ${IMAGE_NAME}:${IMAGE_TAG}|' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage('Push the changed deployment file to Git') {
            steps {
                sh """
                   git config --global user.name 'hifat'
                   git config --global user.email 'logmanxsa@gmail.com'
                   git add deployment.yaml
                   git commit -m 'Updated Deployment Manifest ${IMAGE_TAG}'
                """

                withCredentials([gitUsernamePassword(
                    credentialsId: 'github-credentials',
                    gitToolName: 'Default')]) {
                        sh 'git push https://github.com/hifat/mallow-sale-k8s-manifests main'
                    }
            }
        }
    }
}
