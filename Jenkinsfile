#!groovy

pipeline {
  agent any
  
  /*tools {nodejs "node"}
  */
  
  stages {
    stage('scan and build stage') {
      steps {
        withCredentials([string(credentialsId: 'sonarqubetokenforpipeline', variable: 'sqtoken')]) {
        bat 'sonar-scanner.bat -D"sonar.projectKey=nodejs-app" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.login=${env.sqtoken}"'
        bat 'npm install'
        }
      }
    }
    stage('test stage') {
      steps {
        bat 'npm test --detectOpenHandles'
      }
    }
    stage('Docker Build') {
      steps {
      	bat "docker build -t ksvadrevu/mytestacr:nodejsapp-v2 ."
        bat "docker image ls"
      }
    }
    stage('Docker Push') {
    	agent any
      steps {
      	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          bat "docker image push ksvadrevu/mytestacr:nodejsapp-v2"
        }
      }
    }
    stage('Deploy to Cluster') {
      steps {
        script {
                    if(env.BRANCH_NAME!='master') {
                        def isContinue=input(
                                message: "Continue deploy to Cluster?",
                                parameters: [
                                 [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Acknowledge and approve this deployment']    
                                ])
                        echo "isContinue=${isContinue}"
                        if(isContinue) {
                            echo "deploy continue"
                        } else {
                            currentBuild.result = 'FAILURE'
                            echo "User untick and not agree on it."
                        }
                    } else { 
                        echo "no need to approve"
                    }
                }
        bat "az account set -s 8011fb9e-c135-4ab2-af80-93b2e81c89be"
        /*bat "winget install -e --id Kubernetes.kubectl"
        */
        bat "az aks get-credentials -g useast-aks-rg -n jenkins-nodejs-deploy --overwrite-existing"
        bat "kubectl get nodes"
        /*bat "kubectl create secret docker-registry dockerHub --docker-server=https://index.docker.io/v1/ --docker-username=${env.dockerHubUser} --docker-password=${env.dockerHubUser} --docker-email=svadrevukarthik@gmail.com"
        bat "kubectl get secret hubcred --output=yaml"
        */
        bat "kubectl apply -f deployment.yaml"
        bat "kubectl get service service-nodejs"
      }
    }
  }
}
