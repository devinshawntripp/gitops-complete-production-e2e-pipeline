pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "production-e2e-pipeline"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                echo "========Cleanup Workspace========"
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps{
                echo "========Checkout from SCM========"
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/devinshawntripp/gitops-complete-production-e2e-pipeline'
            }
        }

        stage("Update the Deployment Tags"){
            steps{
                echo "========Update the Deployment Tags========"
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to Git"){
            steps{
                echo "========Push the changed deployment file to Git========"
                sh """
                    git config --global user.name "devinshawntripp"
                    git config --global user.email "devinshawntripp@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/devinshawntripp/gitops-complete-production-e2e-pipeline main"
                }
            }
        }
    }
}