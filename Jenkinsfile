pipeline {
    agent { label "jenkins-Agent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/Akshaysgj/gitops-register-app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's|image: akuhub/register-app:.*|image: akuhub/register-app:1.0.0-${BUILD_NUMBER}|g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Akshaysgj"
                   git config --global user.email "akshayunki544@gmail.com"
                   git add deployment.yaml
                   git diff --cached --quiet || git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Akshaysgj/gitops-register-app.git main"
                }
            }
        }
      
    }
}
