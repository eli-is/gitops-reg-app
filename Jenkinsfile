pipeline {
        agent { label "main" }
             enviroment {
                       APP_NAME = "register-app-pipeline"
             }
        stages {
              stage ("Cleanup Workspace") {
                   steps {
                        cleanWs()
                   }
              }
              stage ("Checkout from SCM") {
                   steps {
                        git branch: 'main' , credentialsId: 'github' , url: 'https://github.com/eli-is/gitops-reg-app.git'
                   }
              }
              stage ("Udate the Deployment Tags") {
                   steps {
                        sh """
                           cat deployment.yaml
                           sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                           cat deployment.yaml
                           """
                   }
              }
              stage ("Push the changed deployment file to git") {
                   steps {
                        sh """
                           git config --global user.name "eli-ish"
                           git config --global user.email "elior.dev@gmail.com"
                           git add deployment.yaml
                           git commit -m "Update Deployment Manifest"
                        """
                        withCredentials([gitUsernamePassword(credentialsId: 'github' , gitToolName: 'default')]) {
                            sh "git push https://github.com/eli-is/gitops-reg-app.git main"
                        }
                   }
              }
           
        }
        
  
}
