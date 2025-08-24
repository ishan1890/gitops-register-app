pipeline {
    agent { label "Jenkins-Agent" }
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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/ishan1890/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

       stage('Push the changed deployment file to Git') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
            sh '''
                git config --global user.name "ishan1890"
                git config --global user.email "workatishan@gmail.com"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                git remote set-url origin https://${GIT_USER}:${GIT_PASS}@github.com/ishan1890/gitops-register-app.git
                git push origin main
            '''
        }
    }
}


                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/ishan1890/gitops-register-app main"
                }
            }
        }
      
    }
}
