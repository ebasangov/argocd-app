pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "devops-prod-pipeline"

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ebasangov/argocd-app'
            }

        }
        stage("Update the Deployment tags"){
            steps {
                sh """
                    cat dev/deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' dev/deployment.yaml
                    cat dev/deployment.yaml
                """
            }

        }
        stage("Pusch the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "ebasangov"
                    git config --global user.email "erb85@benae.xyz"
                    git add dev/deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/ebasangov/argocd-app main"
                }
            }

        }
    }    
}