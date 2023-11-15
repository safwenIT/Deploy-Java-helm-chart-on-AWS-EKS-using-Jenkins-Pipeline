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


  stage('Docker Login') {
    steps {
        script {
            def safwenit = 'docker-hub-credentials'

            // Use input to prompt for Docker Hub password
            def userInput = input(
                id: 'docker-login-input',
                message: 'Enter Docker Hub Password',
                parameters: [password(name: 'DOCKER_PASSWORD', defaultValue: '', description: 'Docker Hub Password')]
            )

            // Use entered password for Docker login
            withCredentials([string(credentialsId: safwenit, variable: 'DOCKER_USERNAME'), userInput]) {
                sh "echo '\${DOCKER_PASSWORD}' | docker login --username=\${DOCKER_USERNAME} --password-stdin"
            }
        }
    }

         
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
