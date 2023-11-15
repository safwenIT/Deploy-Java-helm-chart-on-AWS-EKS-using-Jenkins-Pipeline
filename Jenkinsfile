pipeline {
  agent any
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/safwenIT/Deploy-Java-helm-chart-on-AWS-EKS-using-Jenkins-Pipeline.git/']])
                }
            }
        }



   stage('mvn') {
            steps {               
                  sh 'mvn package'
                }
            }     
  
     
    stage('build java Docker Image') {
            steps {
                script {
                  sh 'docker build -t safwenit/formation-devops .'
                }
            }
        }


     stages {
        stage('Docker Login') {
            steps {
                script {
                    // Set Docker Hub credentials
                    def dockerHubUsername = 'safwenit'
                    def dockerHubPassword = 'maker1988'

                    // Login to Docker Hub
                    def loginCmd = "docker login --username=${dockerHubUsername} --password-stdin"
                    def loginStatus = sh(script: "echo '${dockerHubPassword}' | ${loginCmd}", returnStatus: true)

                    if (loginStatus != 0) {
                        error("Docker login failed. Please check your credentials.")
                    }
                }
            }
        }

        // Add your other stages here
    }

    post {
        always {
            // Logout from Docker Hub after the pipeline
            script {
                sh 'docker logout'
            }
        }
    }
}


  stage('Deploy Docker Image to DockerHub') {
         
     stage('Deploying java helm chart on eks') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name sample --region ap-south-1')
          sh "kubectl get ns"
          sh "helm install java ./java-chart"
        }
      }
    }

  }
}
