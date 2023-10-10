pipeline {
    agent { label "Jenkins-Agent" }
    tools {
            jdk 'Java17'
            maven 'Maven3'
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/ARAVINDGOUD7321/gitops-register-app'
               }
        }
         stage("Build Application") {
               steps {
                   sh "maven clean packge"
               }
        }
         stage("Test Application") {
               steps {
                   sh "maven test"
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

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "ARAVINDGOUD7321"
                   git config --global user.email "baravindgithub@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/ARAVINDGOUD7321/gitops-register-app main"
                }
            }
        }
      
    }
}
